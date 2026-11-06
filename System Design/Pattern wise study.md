### To study

## How to use this
1. For each pattern, study the canonical problem until you can explain the core trade-off from memory.
2. Then practice 2–3 variants, timeboxed to 45 min each.
3. After each attempt, compare against a reference and log gaps **by pattern**, not by question.

| Pattern                              | Problem                                    | Status  |                                       |
| ------------------------------------ | ------------------------------------------ | ------- | ------------------------------------- |
| **Fan-out**                          | 1. News feed / Twitter timeline **(Main)** |         |                                       |
|                                      | 2. Instagram feed                          |         |                                       |
|                                      | 3. LinkedIn feed                           |         |                                       |
|                                      | 4. Facebook groups feed                    |         |                                       |
|                                      | 5. Reddit home page                        |         |                                       |
| **High-write ingestion**             | 1. Ad click aggregator **(Main)**          |         |                                       |
|                                      | 2. Metrics/monitoring (Datadog)            |         |                                       |
|                                      | 3. Log aggregation                         |         |                                       |
|                                      | 4. IoT sensor pipeline                     |         |                                       |
|                                      | 5. YouTube view counter                    |         |                                       |
| **Contention on scarce resources**   | 1. Ticketmaster **(Main)**                 |         |                                       |
|                                      | 2. Flash sale                              |         |                                       |
|                                      | 3. Hotel/Airbnb booking                    |         |                                       |
|                                      | 4. Restaurant reservation                  |         |                                       |
|                                      | 5. E-commerce inventory                    |         |                                       |
|                                      | 6. Movie seat booking                      |         |                                       |
| **Money / strong consistency**       | 1. Payment system (Stripe) **(Main)**      |         |                                       |
|                                      | 2. Digital wallet                          |         |                                       |
|                                      | 3. Bank transfers                          |         |                                       |
|                                      | 4. Stock exchange order book               |         |                                       |
|                                      | 5. Subscription billing                    |         |                                       |
|                                      | 6. Refunds                                 |         |                                       |
| **Proximity search**                 | 1. Uber / ride matching **(Main)**         |         |                                       |
|                                      | 2. Yelp / nearby places                    |         |                                       |
|                                      | 3. Food delivery dispatch                  |         |                                       |
|                                      | 4. Find My Friends                         |         |                                       |
|                                      | 5. Tinder nearby                           |         |                                       |
| **Real-time delivery**               | 1. WhatsApp / chat **(Main)**              |         |                                       |
|                                      | 2. Slack channels                          |         |                                       |
|                                      | 3. Live comments                           |         |                                       |
|                                      | 4. Google Docs collab                      |         |                                       |
|                                      | 5. Multiplayer game lobby                  |         |                                       |
|                                      | 6. Online presence                         |         |                                       |
| **Search / prefix**                  | 1. Typeahead / autocomplete **(Main)**     |         |                                       |
|                                      | 2. Twitter search                          |         |                                       |
|                                      | 3. E-commerce product search               |         |                                       |
|                                      | 4. Google Docs search                      |         |                                       |
|                                      | 5. Contact search                          |         |                                       |
| **Large blobs**                      | 1. Dropbox / Google Drive **(Main)**       |         |                                       |
|                                      | 2. YouTube upload + transcoding            |         |                                       |
|                                      | 3. Image hosting (Imgur)                   |         |                                       |
|                                      | 4. Photo backup                            |         |                                       |
|                                      | 5. S3 itself                               |         |                                       |
| **Async jobs / scheduling**          | 1. Distributed job scheduler **(Main)**    |         |                                       |
|                                      | 2. Notification system                     |         |                                       |
|                                      | 3. Email campaign sender                   |         |                                       |
|                                      | 4. Cron-as-a-service                       |         |                                       |
|                                      | 5. Video transcoding pipeline              |         |                                       |
|                                      | 6. Webhook delivery                        |         |                                       |
| **Top-K / counting at scale**        | 1. Leaderboard / trending **(Main)**       | Ongoing | [[[Tag] Top-K and counting at scale]] |
|                                      | 2. Top-K YouTube videos                    |         | [[[Easy] 0 - Rate Limiter]]                      |
|                                      | 3. Trending hashtags                       |         | [[Ad click aggregator]] -> ongoing    |
|                                      | 4. Game leaderboard                        |         |                                       |
|                                      | 5. Like counter on viral post              |         |                                       |
| **Rate limiting**                    | 1. API rate limiter **(Main)**             |         |                                       |
|                                      | 2. Login attempt throttling                |         |                                       |
|                                      | 3. Per-tenant quota                        |         |                                       |
|                                      | 4. DDoS protection layer                   |         |                                       |
| **Crawling / distributed pipelines** | 1. Web crawler **(Main)**                  |         |                                       |
|                                      | 2. Distributed web scraper                 |         |                                       |
|                                      | 3. Link checker                            |         |                                       |
|                                      | 4. Sitemap generator                       |         |                                       |
|                                      | 5. Data backfill pipeline                  |         |                                       |
| **Unique IDs & sharding**            | 1. URL shortener (TinyURL) **(Main)**      |         |                                       |
|                                      | 2. Pastebin                                |         |                                       |
|                                      | 3. Distributed ID generator                |         |                                       |
|                                      | 4. Key-value store design                  |         |                                       |
| **Caching & read scaling**           | 1. URL shortener redirect path **(Main)**  |         |                                       |
|                                      | 2. Product catalog page                    |         |                                       |
|                                      | 3. Profile service                         |         |                                       |
|                                      | 4. Config service                          |         |                                       |
|                                      | 5. DNS-like lookup                         |         |                                       |
| **Event-driven consistency**         | 1. Order management system **(Main)**      |         |                                       |
|                                      | 2. Inventory sync across warehouses        |         |                                       |
|                                      | 3. Audit log                               |         |                                       |
|                                      | 4. CQRS read models                        |         |                                       |
|                                      | 5. Ledger projections                      |         |                                       |


# System Design Tech Study List

| Category                      | Technology                       | What to know for interviews                                                                                                                                                     | Patterns | Status |
| ----------------------------- | -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------ |
| **Caching**                   | 1. Redis                         | Data structures (sorted set, hash, list, geo); TTL; atomic ops + Lua; distributed locks and Redlock caveats; cluster sharding; persistence (RDB/AOF); single-threaded model     |          |        |
|                               | 2. Memcached                     | Simple KV cache; how it differs from Redis; when it's enough                                                                                                                    |          |        |
|                               | 3. CDN (CloudFront / Cloudflare) | Edge caching; cache keys; TTL vs invalidation; push vs pull CDN; serving static and video content                                                                               |          |        |
| **Relational DB**             | 1. PostgreSQL / MySQL            | B-tree indexes; transactions + isolation levels; row locks (SELECT FOR UPDATE); optimistic locking; read replicas + replication lag; partitioning; when a single node is enough |          |        |
| **NoSQL / NewSQL**            | 1. Cassandra                     | Partition key vs clustering key; LSM tree; tunable consistency (quorum); write-heavy workloads; modeling around query patterns; no joins                                        |          |        |
|                               | 2. DynamoDB                      | Partition + sort key; GSI/LSI; conditional writes; transactions; streams; hot partition problem; on-demand vs provisioned                                                       |          |        |
|                               | 3. Spanner / CockroachDB         | Globally consistent transactions; TrueTime / hybrid clocks; interleaved tables; cost of strong consistency across regions                                                       |          |        |
| **Search**                    | 1. Elasticsearch                 | Inverted index; shards + replicas; near-real-time refresh; relevance scoring basics; syncing from primary DB via CDC; not a source of truth                                     |          |        |
| **Messaging & streaming**     | 1. Kafka                         | Topics, partitions, consumer groups; ordering per partition; offsets + replay; retention; delivery semantics (at-least-once, exactly-once); partition key choice                |          |        |
|                               | 2. SQS / RabbitMQ                | Queue vs log; visibility timeout; DLQ; delay queues; competing consumers; when to pick over Kafka                                                                               |          |        |
|                               | 3. Pub/Sub (GCP) / SNS           | Fan-out to subscribers; push vs pull delivery; ordering keys                                                                                                                    |          |        |
|                               | 4. CDC (Debezium)                | Tailing the DB log (binlog/WAL); outbox pattern; keeping search/cache/analytics in sync                                                                                         |          |        |
| **Stream / batch processing** | 1. Flink                         | Windows (tumbling, sliding, session); watermarks + late events; stateful processing; checkpoints for fault tolerance                                                            |          |        |
|                               | 2. Spark                         | Batch jobs over large datasets; when batch beats streaming; reconciliation jobs                                                                                                 |          |        |
| **Storage & analytics**       | 1. S3 / GCS                      | Presigned URLs; multipart upload; storage classes + lifecycle; eventual vs strong consistency; metadata kept in a separate DB                                                   |          |        |
|                               | 2. BigQuery / ClickHouse / Druid | Columnar storage; OLAP vs OLTP; pre-aggregation;                                                                                                                                |          |        |

### Main Map - more info

| #   | Pattern                          | Crux (what makes it hard)                                          | Numbers that decide the variant                                                       | Solution variants                                                                                                                                    | Canonical problem            | Practice variants                                                                                           |
| --- | -------------------------------- | ------------------------------------------------------------------ | ------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------- | ----------------------------------------------------------------------------------------------------------- |
| 1   | Fan-out                          | Delivering one write to many readers fast                          | Read/write ratio; follower distribution (max followers); feed freshness SLA           | Fan-out on write (push); fan-out on read (pull); hybrid (push for normal users, pull for celebrities)                                                | News feed / Twitter timeline | Instagram feed; LinkedIn feed; Facebook groups feed; Reddit home page                                       |
| 2   | High-write ingestion             | Absorbing a huge write rate without losing data                    | Peak writes/sec; event size; query latency needed (real-time vs batch); retention     | Kafka buffer + batch writes; stream processing (Flink) with windowed aggregation; LSM/time-series DB; Lambda vs Kappa                                | Ad click aggregator          | Metrics/monitoring (Datadog); log aggregation; IoT sensor pipeline; YouTube view counter                    |
| 3   | Contention on scarce resources   | Many users competing for the same limited items; no overselling    | Concurrent requests per item; hold duration; stock size                               | Pessimistic lock (SELECT FOR UPDATE); optimistic lock (version/CAS); reservation with TTL (Redis); virtual waiting queue; pre-sharded stock counters | Ticketmaster                 | Flash sale; hotel/Airbnb booking; restaurant reservation; e-commerce inventory; movie seat booking          |
| 4   | Money / strong consistency       | Correctness over availability; no double spend; auditability       | Transactions/sec; cross-service or single DB; external provider latency               | Idempotency keys; double-entry ledger; saga (orchestration vs choreography); outbox pattern; 2PC (usually avoid); reconciliation jobs                | Payment system (Stripe)      | Digital wallet; bank transfers; stock exchange order book; subscription billing; refunds                    |
| 5   | Proximity search                 | Finding nearby things when locations change constantly             | Location update frequency; number of moving entities; search radius; density hotspots | Geohash; quadtree; H3/S2 cells; Redis GEO; in-memory index vs DB-backed                                                                              | Uber / ride matching         | Yelp / nearby places; food delivery dispatch; Find My Friends; Tinder nearby                                |
| 6   | Real-time delivery               | Pushing messages instantly to online users at scale                | Concurrent connections; messages/sec; group size; offline delivery needs              | WebSockets vs SSE vs long polling; connection gateway + pub/sub (Redis/Kafka); presence service; per-user inbox for offline                          | WhatsApp / chat              | Slack channels; live comments; Google Docs collab (plus OT/CRDT); multiplayer game lobby; online presence   |
| 7   | Search / prefix                  | Fast lookup over large text or prefixes                            | Corpus size; QPS; freshness of index; ranking complexity                              | Inverted index (Elasticsearch); trie with precomputed top-K; offline index build + online serving; caching popular prefixes                          | Typeahead / autocomplete     | Twitter search; e-commerce product search; Google Docs search; contact search                               |
| 8   | Large blobs                      | Uploading, storing and serving huge files reliably                 | File size; upload frequency; read/write ratio; network reliability                    | Presigned URLs to object store (S3/GCS); chunking + resumable upload; content-hash dedup; CDN for reads; metadata DB separate from blobs             | Dropbox / Google Drive       | YouTube upload + transcoding; image hosting (Imgur); photo backup; S3 itself                                |
| 9   | Async jobs / scheduling          | Running work later or reliably, with retries                       | Jobs/sec; scheduling precision; job duration; delivery guarantee needed               | Message queue + workers; delayed queue / time-bucketed tables; at-least-once + idempotent consumers; DLQ; leader election for scheduler              | Distributed job scheduler    | Notification system; email campaign sender; cron-as-a-service; video transcoding pipeline; webhook delivery |
| 10  | Top-K / counting at scale        | Counting or ranking at high write rates; hot keys                  | Events/sec; number of distinct keys; exactness required; time window                  | Sharded counters; Redis sorted sets; count-min sketch + heap (approximate); stream windowed aggregation; periodic precompute                         | Leaderboard / trending       | Top-K YouTube videos; trending hashtags; game leaderboard; like counter on viral post                       |
| 11  | Rate limiting                    | Enforcing quotas consistently across many servers                  | Requests/sec per key; number of keys; strictness (exact vs approximate)               | Token bucket; leaky bucket; fixed vs sliding window; centralized Redis vs local + sync; placement (gateway vs service)                               | API rate limiter             | Login attempt throttling; per-tenant quota; DDoS protection layer                                           |
| 12  | Crawling / distributed pipelines | Processing a huge, unbounded workload fault-tolerantly             | Pages/items per day; politeness limits; dedup volume; failure rate                    | URL frontier with priority queues; per-domain politeness queues; bloom filter / hash dedup; checkpointing; work partitioning                         | Web crawler                  | Distributed web scraper; link checker; sitemap generator; data backfill pipeline                            |
| 13  | Unique IDs & sharding            | Generating keys and partitioning data without hotspots             | Write rate; ordering needed; number of shards; access pattern                         | Snowflake IDs; UUID vs sortable IDs; hash vs range partitioning; consistent hashing; key salting for hot keys                                        | URL shortener (TinyURL)      | Pastebin; distributed ID generator; key-value store design                                                  |
| 14  | Caching & read scaling           | Serving hot reads fast while keeping data fresh enough             | Read QPS; hot set size; staleness tolerance; cache hit ratio                          | Cache-aside; write-through / write-behind; CDN; read replicas; thundering-herd protection (request coalescing, jitter)                               | URL shortener redirect path  | Product catalog page; profile service; config service; DNS-like lookup                                      |
| 15  | Event-driven consistency         | Keeping multiple services in sync without distributed transactions | Events/sec; ordering requirements; tolerance for eventual consistency                 | Outbox + CDC; event sourcing; idempotent consumers; ordering by partition key; replay / rebuild projections                                          | Order management system      | Inventory sync across warehouses; audit log; CQRS read models; ledger projections                           |
