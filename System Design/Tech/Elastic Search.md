## Overview
Elastic search is an engine which provide search and analytics capabilities over documents.

## Internals
Elastic Search in itself is a cluster management system, while the real algorithm of searching is happens using Apache Lucene. 
There are nodes in the ES cluster having different responsibilities, one is *control plane*: managing the cluster nodes; data plane: serves the user queries and stores the data.

###### Data plane:
contains, Data Node and Coordinating Node. Data stays in the data node, and coordination of retrieving the relevant data happens in the Coordinating Node. There is also Ingest(ML) node. You can read it if needed.
Coordinating node gets the user query and then it goes to data nodes to retrieve the relevant data, then merges the data and returns.

```
                        CLIENT
                          │
                          ▼
              ┌───────────────────────┐
              │  COORDINATING NODE    │  ← entry point, query planning,
              │  (the "front door")   │    scatter-gather, result merge
              └───────────┬───────────┘
                          │ fan out
          ┌───────────────┼───────────────┐
          ▼               ▼               ▼
     ┌─────────┐    ┌─────────┐    ┌─────────┐
     │  DATA   │    │  DATA   │    │  DATA   │  ← store shards, execute
     │  NODE   │    │  NODE   │    │  NODE   │    queries locally
     └─────────┘    └─────────┘    └─────────┘
          ▲
          │ writes flow in via
     ┌─────────┐
     │ INGEST  │  ← transform/enrich pipeline before indexing
     │  NODE   │
     └─────────┘

     ┌─────────┐
     │ MASTER  │  ← cluster state only: create/delete indices,
     │  NODE   │    add/remove nodes, shard allocation.
     └─────────┘    NOT in the data path.
```

In ES most of the documents inserted are immutable and that is how the ES performs. Updation here is more heavy than inserts. Updation requires, soft delete and then once these all deleted. As ES is not designed for updation, we can make use of caching on nodes for better throughput.

```
CLUSTER
 └── INDEX  ("books")                    ← logical, what you query
      ├── SHARD 0  (primary)             ← physical, one Lucene index
      │    └── Lucene Index
      │         ├── Segment 1  [immutable]
      │         │    ├── inverted index
      │         │    ├── doc values
      │         │    ├── stored fields (_source)
      │         │    └── BKD trees (numerics/geo)
      │         ├── Segment 2  [immutable]
      │         └── Segment 3  [immutable]
      ├── SHARD 0  (replica)             ← exact copy, on a different node
      ├── SHARD 1  (primary)
      └── SHARD 1  (replica)
```

#### Shared and Replicas
ES maintains shards for storing the indexes and the documents. It stores a segment within each shard. Replicas are copy of these shards and helps with scale. The number if shards are fixed at the config time, it is important to keep it as close as possible to your use case, and changing the number of shards
###### Shards
Shards are the ones storing a segment(mini indexes -> inverted index, doc fields etc.). Number of shards are configured at the start up time and changing this number later means a migration. More shards than required means coordinating node needs to fan out, and less means scaling issues. Shares define the storage and write capacity of an ES cluster. 
###### Replicas
Each share have replicas to serve read traffic. These replicas help scale ES, as ES is generally read-heavy. Replicas live on different nodes than primary.
Also node that the data is stored on the disk, so recovering data is easy. Replicas help with availability and throughput and read latency. Also, as for every write, ES cluster replicates, more replicas mean slower writes.


## Write path

Request goes to any node, which will act as coordinating node. Finds the primary shard of the request. Then the write is written to cache of primary and to a disk WAL. Then this update goes to replicas. Once every replica acknowledges, request is successful. Still the document is not searchable. 

Every second ES flushes the memory to filesystem cache (still not disk) -> becomes searchable.
Periodically, this FS cache is flushed to disk, and segments are created/merged. This operation is IO and CPU heavy so this is not done very frequently. Merges of segments results into smaller number of segments, which means less places to search for => better read performance.
```
Client → Coordinating node → hash(_id) % shards → Primary shard
                                                       │
                                            ┌──────────┴──────────┐
                                            ▼                     ▼
                                      buffer + translog      replicas (parallel)
                                            │                     │
                                            └────── ack ◄─────────┘
                                            │
                                   [~1s later] refresh → segment → searchable
                                            │
                                   [background] flush → disk
                                   [background] merge → consolidate
```

## Read Path

Read happens in 2 steps: 
1. Query (find the relevant ids)
2. Fetch (get the relevant documents)

Say we are looking for a keyword 'Book', price under $15, and results should be sorted:

###### Query

```
                    CLIENT
                      │  "Great", price<15, sort by price, size=10
                      ▼
            ┌──────────────────────┐
            │  COORDINATING NODE   │
            │  • parse query       │
            │  • plan execution    │
            │  • pick ONE copy per │
            │    shard             │
            └──────┬───────┬───────┘
                   │       │  fan out to ALL shards
         ┌─────────┘       └─────────┐
         ▼                           ▼
   ┌───────────┐               ┌───────────┐
   │ SHARD 0   │               │ SHARD 1   │
   │ (primary) │               │ (replica) │
   ├───────────┤               ├───────────┤
   │ search    │               │ search    │
   │ every     │               │ every     │
   │ segment:  │               │ segment:  │
   │           │               │           │
   │ inverted  │               │ inverted  │
   │ index →   │               │ index →   │
   │ matching  │               │ matching  │
   │ doc IDs   │               │ doc IDs   │
   │     ↓     │               │     ↓     │
   │ BKD tree  │               │ BKD tree  │
   │ → price   │               │ → price   │
   │   filter  │               │   filter  │
   │     ↓     │               │     ↓     │
   │ doc values│               │ doc values│
   │ → prices  │               │ → prices  │
   │   to sort │               │   to sort │
   │     ↓     │               │     ↓     │
   │ local top │               │ local top │
   │    10     │               │    10     │
   └─────┬─────┘               └─────┬─────┘
         │                           │
         │  returns ONLY:            │
         │  [doc_id, sort_value]     │
         │  — no document bodies     │
         └───────────┬───────────────┘
                     ▼
            ┌──────────────────────┐
            │  COORDINATING NODE   │
            │  merge 20 candidates │
            │  → global top 10     │
            └──────────────────────┘
```

###### Fetch

```
            ┌──────────────────────┐
            │  COORDINATING NODE   │
            │  I need the bodies   │
            │  for these 10 IDs    │
            └──────┬───────┬───────┘
                   │       │  multi-get, only relevant shards
         ┌─────────┘       └─────────┐
         ▼                           ▼
   ┌───────────┐               ┌───────────┐
   │  SHARD 0  │               │  SHARD 1  │
   │  read     │               │  read     │
   │  _source  │               │  _source  │
   │  (stored  │               │  (stored  │
   │   fields) │               │   fields) │
   └─────┬─────┘               └─────┬─────┘
         └───────────┬───────────────┘
                     ▼
              full JSON docs → CLIENT
```

##### Pagination

Deep pagination can de expensive, say the offset is 10k and the size is 10. Total shards are 10;

Each shard in that case need to prepare 10010 document ids. The coordinating node will merge all those -> 10010 x 10 -> to get the top 10.
That is why we have search_after???

## Trade offs

| Dimension                  | What you get                                                            | What it costs                                                                                                |
| -------------------------- | ----------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| **Inverted index**         | O(1)-ish term lookup instead of O(n) scan                               | Full duplicate copy of your data, restructured; index build time on write                                    |
| **Immutable segments**     | Lock-free reads, aggressive caching, great compression, simple recovery | Updates cost more than inserts; continuous background merge CPU/IO; temporary 2x disk during merges          |
| **Sharding**               | Scale past one machine; parallel query execution                        | Every search fans out to all shards (latency = slowest shard); primary shard count nearly frozen at creation |
| **Replication**            | HA + multiplied read throughput                                         | Write latency bounded by slowest replica; storage multiplied by (1 + replica count)                          |
| **1s refresh interval**    | Huge write throughput vs per-write fsync                                | ~1s window where written data isn't searchable — real eventual consistency                                   |
| **Denormalized documents** | Answers in 1–2 queries, no joins needed                                 | Write-side transformation logic; data duplication; update amplification when a shared entity changes         |
| **Doc values (columnar)**  | Fast sorting/aggregation without reading full docs                      | More disk; `text` fields excluded by default (needs a `keyword` sub-field)                                   |
| **Rich query DSL**         | Fuzzy match, geo, faceting, relevance ranking out of the box            | Verbose JSON; a genuinely deep learning curve; easy to write accidentally expensive queries                  |

## Problems

#### Sync Drifts with db

Say we want to write to the db as well as to ES, but doing that in one block raises the problem of [[Dual Writes]], which means that while db shows something else, ES might show some other value. 

Even when we want to use transactions, ES does not really have it, in any case, any given code statement can fail at any time. So, we want to avoid these dual write paths. Making two systems write atomically creates more problem. For example, a timeout cannot be classified as success/failure.

So, we do this by say, using a CDC, which will write in order in ES, we can use Debezium for WAL(Later we can rebuild the ES indices using it). We can also add a version, just fencing token, so always the latest write wins, and stale writes can be avoided.
One other way is to use an outbox table, a poller can just update ES.
We can also keep on running the reconciliation job.

#### Mapping explosions

In case of a lot of dynamic mapping, creating different fields each time, can create hard to search indexes. 
Do not keep the dynamic mapping, or keep it strict.

#### Frequent updates

As pointed above, ES soft deletes and reinserts for updating. 
If in case, updates are too many per second, it can cause a lot of tombstone entries, because of which searching will be impacted, and merges will be heavier on CPU and IO.

In case of heavy update workloads, better to keep volatile values in some other store, e.g. [[Redis]].
Or if it is not possible then better to batch you updates.

You can also use \_update which will do a partial update and not fetch a complete doc to change its value.

#### Pagination

Same as pointed above if a deep page is requested then each shard will create the response with offset+size records, all of these will be sent to coordinator where they will be merged, this is a heavy operation to do.

How to solve: `search_after`

This woks as pagination in SQL, after say another page is requested to the coordinator node, we send down an identifier to all the data nodes, starting which they should next share the top 10 values. This identifier should be recognised by all the shards so, they do not repeat any record.

Point in time `search_after`, creates a snapshot in all data nodes, so even if the indexes are being added/removed, the data does not shift.
According to your needs, you can set the timeout for these snapshots.

#### Hot Shard

It is quite self explainable.

Solutions include, distributing the hot key, defining the key well. Add more resources for the whale.
Whatever stores a key:value, gets this issue of hot shard.

In case of ES we have liberty to distribute the key among the shards, so now for a hot key fan out will happen to these dedicated shards (not whole cluster).

In case of read-hot shard, we can always add more replicas.

## General use case

```
   WRITE PATH                                    READ PATH
   ──────────                                    ─────────

   Client                                        Client
     │ write                                       │ search
     ▼                                             ▼
 ┌────────────┐                              ┌──────────────┐
 │  Postgres  │                              │ Elasticsearch│
 │  DynamoDB  │◄─── source of truth          │  (derived,   │
 │  Spanner   │                              │  disposable) │
 └─────┬──────┘                              └──────▲───────┘
       │ CDC                                        │
       │ (Debezium / DynamoDB Streams /             │
       │  Spanner change streams)                   │
       ▼                                            │
 ┌────────────┐                                     │
 │   Kafka    │─────────── consumer ────────────────┘
 └────────────┘
```

##### E-commerce product search

Say you have 50 mil products and 5k req/sec
Requirement: Typo search, relevance sorting, structured attribute search, paginate

If all this is needed then ES is a good option. A db can support these but not all at the same time.
We can index the searchable fields of a product. 

```
Postgres      → products, categories, sellers, prices (source of truth)
Elasticsearch → the denormalized search document
Redis         → view counts, cart counts, session data, hot-query result cache
```

Somethings to remember
- A renaming, say category, need to touch a lot of documents in ES, have a bulk query job for this purpose.
- Cap page size, so we are not falling into deep pagination which will use the system very heavily.

##### Proximity Search

Let us search by providing the radius and lon/lats.
We can also make use of PostGIS, it also gives us transactional guarantees.
The only thing is for large scale, full text search of postgres is not great. For small set, it is really great. 

When search plus fuzzy search and relevance ranking is needed, lean towards ES.
Elasticsearch supports various geospatial indexing strategies including geohashing, quadtrees, and R-trees, allowing for efficient location-based searches. It also excels at full-text search and category filtering, making it ideal for our multi-faceted search requirements.

##### Log and metric analytics

This is what ES is made for. Logs are immutable, and ES works best with it. No soft-deletes, no tombstones, no merge pressure, just consolidations.

ES makes it possible to search in the logs. We generally partition the logs using the ts, as it helps us maintain the shards. We can have hot/warm/code data nodes. Hot node will accept the writes, and keeps a replica. Warm node, will have the recent logs and they will be consolidated in one segment. Then cold nodes, will have older data, which is searchable but this is rarely queried. A delete node, which will delete data months old.

Also, if your data requires aggregation, ES won't help, you need columnar db. ES is good for text based search.

### Example: When not to use ES?

##### Primary Db

- No transaction support
- read your own write, not possible: ~1s eventual consistency
Better alternatives: Postgres, Dynamo Db, Spanner

##### Rapid changing data

For example the like counts.
Win: Redis

##### Id lookups

No usage of the true strength of ES.
Say, if you have ES already deployed, and only modest Id look up queries are present then okay.

Better alternatives: Redis, DynamoDb

##### Financial Ledger

Breaks here.

- Do not use for OLAP types aggregations.
- Do not use when joins are needed as ES does not support.


## Alternatives

```
  How much data / how complex is the search?

  < 100k docs, simple filters
       └──► Just query your primary DB. Add an index. Done.

  < 1M docs, need full-text search
       └──► Postgres full-text (tsvector + GIN index)

  Need typo tolerance / fuzzy matching, still modest scale
       └──► Postgres pg_trgm, or Meilisearch / Typesense

  Millions of docs, complex relevance, facets, geo
       └──► Elasticsearch / OpenSearch

  Heavy analytical aggregation, not really "search"
       └──► ClickHouse, Druid, BigQuery
```


### How to use ES in an interview?

Do not jump onto ES directly, ask what type of search.

- Under mil products, If mostly filtering -> Postgres GIN index
	- if more than a mil and fuzzy matching: Postgres will struggle on relevance tuning and won't scale that read load horizontally
- Requirements say fuzzy matching, faceted counts, geo-radius, and relevance ranking across 50 million listings -> ES
  So I'd bring in Elasticsearch, fed by CDC off Postgres so I only have one write path and can rebuild the index by replaying the log.
  
  The costs I'm accepting: roughly a one-second staleness window from the refresh interval, sync drift as an operational risk, and I have to denormalize the write side. Given that product search is read-heavy and slightly stale results are fine, that's the right trade."

> **The structure that reads as senior:** _constraint → why the simple thing fails → the choice → what it costs you → why that cost is acceptable here._

## Questions
- What is search after?
- How does ES does a sentence search? if it only tokenizes a word?
- If in a scenario when I need search capabilities on texts but when ES is an overkill, what other options do I have?
- How ES works for auto-completion?
- Can we lose data in ES?
- Query planner in ES