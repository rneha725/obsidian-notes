## Problem
When we have some high traffic and we hit a wall with db, we might need to handle a lot of write traffic, throughput and contention.
- Write Traffic - requests are not getting dropped.
- Throughput - System does not have a bottlenecks
- Contention - either change design or handle it better - [[[Pattern] Dealing with Contention]]

Ways of handling high writes:
- Vertical Scaling and better db choices
- Handling write traffic with queues
- Partitioning and Sharding
- Batching and hierarchical aggregation

## Solutions
- **Better Databases**: Generally in systems, if we want to optimize for reads, we will degrade the writes and vice versa. When selecting the right db, we can optimize for writes and degrading the reads. A good example of this type of db is [[Cassandra]] having its own downsides. But for writes, it has a 100k per second throughput which is commendable. Make sure that the scale requires this. Same goes with any tsDb. Say if you are already fixated on a db, try to see if we can make writes simpler by not having to query other tables, we can also use the batching in db, where disk flush can be done later. 
- **Sharding and Partitioning**: We can always divide the data in shards to serve the write traffic. Sharding requires us to understand our query patterns well. We try to limit accessing a lot of shards per operation. While mentioning sharding it is important to discuss the partition key and understand how our operations will work on them. while sharding can help with both reads and writes, there are other complexities it adds.
	- We also have vertical sharding, basically having different dbs for different concerns. Example, when we need strong relations vs when we store analytics data.
- **Using Queue to handle burst traffic**: While every day is good enough with above solutions, what about a black Friday sale? Queues can be good for shirt lived traffic, your db should be able to handle the steady traffic, after all, queues can pile up if the readers are not able to read at the speed of production. We can have more and more consumers yes.
- **Load Shedding**: Drop the low importance write at the time of spiked traffic, e.g. location sharing services, analytics.
- **Batching**: Instalments of writes.
	- *App layer*: In cases where our application is not the SOT, we can use app layer batching. Say app consumes from Kafka then forwards the batched writes to db, replay in case of app crash. If app is the SOT, then we will lose the batched data when app crashes.
	- *Accumulator service*: Basically reduce the data and forward it. 
	- *DB Layer*: Rarely used, tune the flush time.
- **Hierarchical Aggregation**: Say we have a live stream and people are commenting, each comment needs to get to all the people connected. At the same time, we have like count, likes should be shown to each user connected. 
	- As we can have 1 million users connected, all commenting, 1mil x 1mil requests needs to be processed. This is called **fan-in/fan-out** problem, too many write coming, too many reads going out. This is where we can use this technique.
	- For comments, get all and then forward them to broadcast node, each node is connected to a subset of live stream user. So no from 1mil outbounds we have N outbounds.
	- For like we can aggregate them and use the same strategy to broadcast likes.

## Examples
- **Instagram/Social Media** - Perfect for demonstrating sharding by user ID for posts, vertical partitioning for different data types (user profiles, posts, analytics), and hierarchical storage for older posts.
- **New Feed**: When celebrities post, it goes to follower's feed, and when those followers come online, they see those posts.
- **FB Search**: Search is write-heavy because **every post and like fans out into many index updates**, done in advance so reads are fast. Partitioning spreads that load. Batching shrinks it.
- **Live Comments** - Live comments are a great example of a system which can benefit from hierarchical aggregation to avoid an intractable all-to-all problem where millions of viewers need a shared view of the activities of millions of their peers.

## Deep dives
- How to add a new shard in the production?
	- Gradually, write to both new and old shard and let the read requests go to the new shard.
- Hot key reads and writes handling?
	- Fixed splits of each key to different shards: easy to write, reads need to check every k shard. -> most prod systems use it.
	- Dynamic split: where writers can decide if a key is hot or not, we need a way for readers to know, readers can always check sub key -> wasteful but simple, writers can announce new shards -> more complicated but efficient.

## Quiz