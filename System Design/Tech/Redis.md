## Overview

Redis is an **in-memory key-value store, which is also single threaded**. As all the operations are performed one-by-one, locking is not an issue. Also, it is single threaded, so we do not need multiple cores to run it.

Redis is in-memory, but still it has some durability modes, e.g. snapshots and interval. We can lose some data while a primary fails.
Redis is fast enough to serve 100k/sec writes.

## Pros/cons nad tradeoffs of Redis
##### Pros
- Speed and performance
- flexible data structures
- built in ttl
- Available and good scaling
- Simple queries
- Provides persistence options: snapshots and interval
- Scalability by horizontal scaling, more read replica increases read throughput. 
- Eventual consistent, read [[[TODO] Consistency Guarantees#Eventual Consistency]]
##### Cons
- Volatile storage, it provides eventual consistency.
- joins or complicated queries which require talking to more than one node is not allowed.
- Single thread means that if some operation takes more time, other operations need to wait.
- More expensive per GB as compared to db
- Complicated DS creates more overhead at scale as compared to raw bytes
- Re-sharding, cross-slot operations, and split-brain scenarios add operational complexity -> is it because we are guaranteeing eventual consistencies

##### Tradeoffs

| Give up                                                                    | Gain                                                             |
| -------------------------------------------------------------------------- | ---------------------------------------------------------------- |
| Durability (async replication, no cross-shard ACID)                        | Sub-millisecond latency, huge single-node throughput             |
| Complex queries (no joins/secondary indexes)                               | O(1)/O(log n) predictable ops via purpose-built structures       |
| Simple linear scale-out (single-thread/node, cross-slot limits in Cluster) | No lock contention, deterministic execution, simple mental model |
| Memory economics (RAM-priced storage)                                      | Speed — no disk I/O on the read/write path                       |
| Strict cross-replica consistency                                           | Read scalability + high availability                             |
## Internals

- **Storage format: kv store**: Redis stores by using a key-value configuration. Where key is a simple string while the value can be a complicated DS, like sorted sets, pq, maps, text, streams and even geospatial indexes.

- **Client Connection**: Redis can work as a single machine(with or without replica) or it can be configured as a cluster, when cluster based, it maintains the cluster by using [[[TODO] Gossip Protocol]]. Each node maintains a map of slot to node. While slots are fixed: 16,384. Each key is hashed to a slot and slots have assigned nodes. In a lot of cases, this mapping is also maintained by the client, so they can directly connect to the desired node. Also, a client randomly connects to a node in the cluster.

- **No query router**: Redis does not have a query router so you cannot process one query on multiple machines. That is why it is important to configure the keys cautiously on Redis.

- **Replication**: Shards also maintain a copy of keys for "some" of the other nodes for replication. We define the sync/async replicas at the time of cluster creation.

## Use cases of Redis?
### 1. Redis as Cache

Redis guarantees that you never read a key for which the TTL is expired. Also, you need to define an eviction policy, expiration only handles the staleness it does not evict data in case memory is full. Also, key configuration is important in case [[Hot key problem]] can happen.

In a cluster, for any request, example `GET foo`, gets connected to a node in Redis, which calculated the key slot. If the slot is owned by the node itself, it is returned. Otherwise Redis returns `MOVED` with the correct location. This goes back to the client instead of Redis doing server-side forwarding, this is to keep Redis fast.
Now it is client's work to request again.
In some libraries, client maintain their own slot to node mapping, so it calculate the slot by itself and connects to the node. In case, the node has been rebalanced and keys are moved, the client side map is also updated.

Problmes:
- cold start: thundering herd
- [[Hot key problem]]
- [[Cache Problems#Cache Stampede]] on expiry

### 2. Redis as a distributed lock
You can create a lock, say, using this

```lua
SET lock:concert:343 my-service-token NX EX 30

----
## key-> lock:concert:343
## value -> my-service-token 
```

The NX set succeeds only when the key does not exist. When we want to release the lock, we release by checking if the key exists and the value is my service's token. This second checks saves us from deleting the lock acquired by another service.

Locking in redis is pessimistic and for optimistic lock we need to do a watch, but anyway due to async replication in redis, we still cannot implement optimistic locking that well, better to use sql `for update` for that to checking the version.

##### Problems
- Process A acquires the lock and sets the TTL, A pauses and lock gets expired. Another process B gets the lock and writes to DB, A comes up and now writes again.
	- this is the problem with lease based lock.
	- Solution: Use a fencing token when updating the same resource.
- Clock differences
Better to use chubby or zookeeper for locks.

### Redis for leaderboards

We can use redis priority queue(sorted sets). Maintain the key, that's it. We can always maintain upto top k items in the set.
##### Problems
- Ties brakers needed
- in case size increases, we need to reshard.

### Redis for rate limiting

Sorted sets for dynamic rate limiting. 
For fixed window rate limiting, a normal counter with the expiry, set the expiry only the first time.

##### Problems
- Burst at the tail and head of the window. Not a redis problem but an algo problem.

### Redis for event sourcing

Redis supports append only ds, with no expectations on durability. There are one line commands to add the stream and similarly for reading. 
Streams get processed by consumer groups, containing workers. Workers ack once they are done processing a message. On consumer group crash, it can always resume where it left off. But in case of a slow worker, we do not know how to differentiate between slow and dead, so another worker can always claim another slow/dead workers work. So a message can be processed twice.

### Redis as pubsub

Hard to understand this. Few points are still no durability, and connections are not per channel but per node, so millions of channels do not mean millions of connection.

## Solution to hot key problem in Redis

- **Use key suffixes**: Hash using the key and the suffix, suffix can be something which can be randomised. This is how this key will appear on different nodes and will help us with the traffic distribution. Problem, multiple copies, hence multiple updates. Also, clients needs to know for which keys this flow is used.
- **Cache of the cache**: Maintain the in-client cache for the hot keys, thing is we will get stale values will this client cache expires. This is good for short-lived TTLs.
- **Read replicas**: Good for read-hot key, but for write-hot key, problem is the same.

## When not to use redis
- When you need complex queries.
- When durability is needed. Even writes can vanish.
- When working set is small enough to fir in-memory.
- And when you need durable, replayable streams with long retention for many independent consumers, that's Kafka's job.

### Mention
[[N + 1 Problem in SQL]]
[[[TODO] Gossip Protocol]]
[[[TODO] Cache Patterns#Cache Aside]]

### Few redis applications
###### 1. 100 API calls in an hour

We can use a counter for which will expire. We will use one query which sets the expiry on the first time increment of the counter. We can create a Lua script which will be run as single statement:

```lua
SET key 0 EX 3600 NX - we will increment only the first time.
```

As redis is single threaded, there is no need to acquire lock, and Redis can expire this, so no need of a cron job either.

###### 2. Show the top N players by rank

We can make use of sorted sets here which are [[Skip List]] internally.
Tradeoff: keeping the leaderboard in-memory, we can lose it.

This top k is is a lot faster(log n) then sql order by query(n).

###### 3. last 10 viewed products

Inserting into a list, and according to the value of k (top k), it can be trimmed. A list can still have duplicates, sorted set can be used.
Better than db storage.

###### 4. How many unique visitors does the site have?
Redis has [[HyperLogLog]] which gives approximate unique ids.
A Set would work exactly, but storing 50M user IDs costs gigabytes. HLL gives you ~0.81% error using **12KB regardless of cardinality**. You're explicitly trading exactness for a ~1000x memory saving.

###### 5. Check if the username is taken?

Redis Bloom filters are there. Saves [[Cache Problems#Cache Penetration]]

###### 6. Flash sale - do not oversell?

```lua
-- atomic: check and decrement together
local stock = redis.call('GET', KEYS[1])
if tonumber(stock) > 0 then
  return redis.call('DECR', KEYS[1])
else
  return -1
end
```

Again these needs to be updated to DB.
Redis serializes the concurrent claims on its single thread, bounding how many requests reach the DB and avoiding lock contention. But a cache can be lost on failover — inventory truth must live in the durable store, with reconciliation if Redis fails mid-sale.

###### 7. Who is currently online?

Have a counter, expire 30 seconds. Increase this 30 seconds to next 30 seconds one the heartbeat of a client is received. Otherwise expire.

### When not to use Redis
- No redis stream if you need durability, replay and consumer groups.

### Questions
- Check how redis expires
- how exactly sorted set increase the value and also keep values sorted. -> [[Skip List]]
- is there anything which is restricted as Lua script, any limitations?
- Multiple keys? how to configure
- Do we have replication factor in redis?

### Additions
- Add numbers [[Numbers to know#Redis Numbers to know|Redis Numbers]]
- Lua script restrictions

### Problems
- For a rate limiter, they key space is sparse but traffic is 1M ps, we need multiple Redis instances, but we cannot distribute one key to multiple instances.
	- My understanding is not correct, per user requests are not going to be in millions. They are a small set.

