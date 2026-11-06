### Overview
Cassandra is a distributed noSQL db. It is a partitioned [[[TODO] Wide Column DB]]. Also, it is highly available db, with tunable consistency, for that it used masterless cluster management with consistent hashing and [[[TODO] Gossip Protocol]]. It sacrifices rdbms types joins/queries for performance.

### Properties of Cassandra
- NoSQL wide column db
- Quorum based cluster, LWW
- De-normalised data and simpler queries only, data does not have any foreign key
- Supports a lot of column types
- Atomic isolated writes

### Internals
```java
Map<RowKey, SortedMap<ColumnKey, Value>> 
//columns are sorted according to the column key

//row key determines which node data lives on
```


Primary key = partition key + clustering key

- Partition key: (row key) on which node should data go
- Clustering key: sorting within the partition

##### Scaling
Scales by partitioning the data on many nodes. Uses [[[TODO] Consistent Hashing]].

##### Replication
Replicates within the hash ring, different nodes are picked for replication.

##### Consistency
Flexible, define how many replicas you want the write to go to.

##### Query routing
Any node can receive a query and it routes to the correct node with the key.

##### Storage Model
Uses [[Log Structured Merge Trees (LSM trees)]], LSM trees have commit logs for durability and memtable. So Cassandra has better write speed than read speed.

|                      | LSM tree (Cassandra)                                                               | B-tree (Postgres/MySQL)                                                                      |
| -------------------- | ---------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| **Writes**           | Sequential, append-only — no seek, no in-place update, no read-before-write        | Often requires seeking to the right page, possibly reading it first, then rewriting in place |
| **Write throughput** | Very high — this is the whole point                                                | Lower — random I/O, lock contention on the tree structure                                    |
| **Reads**            | Potentially slower — may need to check memtable + multiple SSTables, merge results | Fast — one structure, always current, one seek path (with index)                             |
| **Space**            | Needs compaction to reclaim space from overwrites/deletes                          | Reclaimed immediately (page is overwritten in place)                                         |
| **Deletes**          | Tombstones — deferred cleanup, can cause read slowdown until compacted             | Immediate — row physically removed/marked free                                               |
|                      |                                                                                    |                                                                                              |

##### Cluster Communication
[[[TODO] Gossip Protocol]] for liveliness checks.

##### Fault Tolerance
In case a node fails, we detect it using [[[TODO] Gossip Protocol]] (Phi Accural). Till a node is recovered, Cassandra uses hinted handoffs to store the data for the failed node, once it is recovered this data is transferred to the correct node.


### Pros/Cons and Tradeoffs 



### When to use Cassandra

### Use cases
- Discord messages
	- For message storage durability, we can serve data in the ts order, as most Discord queries are that.
- Insta feed
	- For normal people, write to Cassandra and use it as materialized view for the feed. Fan out on write
	- For celebrity, fan out on read
- Notification service
	- Write on Cassandra for all the notifications, use a third party gateway to deliver
- Metric service
	- Use as ts db.

### Guarantees
- Flexible consistency, eventual by default
- AP > CP
	- No leader, any node can accept a read and write.
	- Conflicts are resoled by last-write-wins
	- Hinted handoffs when the replica is completely offline
- High write throughput
	- Reads are also scalable but Cassandra is built to support high write throughput and uses [[Log Structured Merge Trees (LSM trees)]] for that

### Tradeoffs
- Eventually consistent
- Simpler queries, does not support joins etc, denormalized data only, single partition queries are needed.
- Last write wins, it can update to a stale write
- Cassandra does not have transaction for performance guarantees.
- Cassandra has uperts not inserts, LWW. -> Writes are faster.

### When to choose Cassandra
- See the access pattern
	- Partition key -> for node storage
	- Clustering key -> for sorting within a partition
	- Partition Size
	- Denormalised data
- Av > Consistency (flexible)

### When not to use
- When you need recent aggregation, go to some stream processor like [[Flink]]

#### Example
- Messaging Apps: Discord messages
	- We need to query for a channel, and the recent messages
		- As Cassandra can store sorted data, we can store using -> (channel_id -> pk, message_id ([[Snowflake Id]])-> ck). We can bucket the data according to the number of days, so 10 days messages can be stored in a node, and so on.
- Insta Timeline:
	- Cassandra can store the materialised feed for the normal users on fan out.
- Ride Sharing:
	- Trail of location Redis -> Cassandra
- Inventory product page:
	- For storing the denormalized product page data
- Recommendation Systems:
	- Kafka -> Cassandra -> ML pipeline
- Notification system:
	- Event -> notif service -> Cassandra -> once sent, mark as sent
- Metrics or observability platforms:
	- hosts -> metric collection -> Cassandra (or ts db) -> query API -> dashboards

