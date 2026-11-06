### Reads
We have b-tree indexes, so reads are faster, but more index can make writes slower.
Btree indexes are good for exact matchs, range queries and sorting. [[[TODO] B-tree indexes]]

### GIN indexes (Generalized Inverted Index)
- Used for full text searches

##### GIN vs [[Elastic Search]]:
ES has:
- relevance sorting
- faceted search capabilities
- fuzzy search
- very large data set search
- Advanced analytics and aggregation
GIN:
- can have upto 10mil documents

### PosGIS
- Perfect for low scale Uber.

### Numbers
- writes: 5k per second per core -> shard if more throughput is needed
- complex transaction: 100 per second

#### Improving write performance
1. Batch processing -> crash prone
2. Vertical scaling
3. Write offloading: use queues, workers to batch
4. Table partitioning
5. Sharding

### Replication
#### Async:
- Write to primary, faster but we can lose the data

#### Sync:
- also writes to replica, slower writes, but more durable and consistent

### Scaling reads
- Add more read replicas, makes it more available

### Consistency
- Supports ACID transactions.
- have different levels of [[[TODO] Isolation Levels]]

### How to use in an interview
- Start from postgres and check for the bottlenecks
	- in case of high write volume try [[Cassandra]] or [[Redis]
- Global multi-region requirements
	- Postgres is single primary
	- [[Cockroach DB]] for global ACID
	- [[Cassandra]] for eventual consistency
	- [[DynamoDb]] for global tables
- KV access patterns:
	- [[Redis]]
	- [[DynamoDb]]
	- [[Cassandra]] for heavy writes