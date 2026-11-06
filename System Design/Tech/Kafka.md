## Overview
Kafka is a distributed event streaming platform, can be used as message queue or stream processing system.

## Terminologies
##### Partitions

A partition is a physical grouping of messages. Partitions have the messages appended to them in order they are received. Consumers in a consumer group connect to partitions. Partitions help system to scale. Also, we can define a partition key to maintain ordering.

A partition is an append log. This is a central part of the kafka cluster. It helps scaling as you can increase the partitions to write more. Immutable and we do not need to change anything which is already written, replication is a bit easier.

##### Consumer groups and consumers

Consumer group consume data from a topic. They can have multiple consumers, data is processed to maintain the ordering within the partition. One consumer per parttition.
Also, consumers maintain an offset in a partition, so in case they crash, they can continue, but a consumer can die before it commits, so the default kafka guarantee is at-least once delivery.
It can be made exactly once by: idempotent producer + transactional consumer.

##### Broker

A broker is just a kafka server.
##### Topic

 A topic is a logic collection of partitions and serves related messages, they are just a way to organize our data better. Topics can have multiple produces and consumer groups. 
 A topic can have multiple partitions and any partition can be present on any broker.

##### Producer-Consumer

Producer produces to a topic and then it is published to a partition according to the key. If key is not provided, kafka uses a default strategy much like round robin, or if something is provided in the producer config. Message has these fields: payload, headers, key, timestamp. First a message is assigned a partition, producer then forwards the message to the broker that has the partition, this info is the kafka controller.

Consumers pull messages from kafka, they poll the messages. This is deliberate to let the consumers have more control over consuming.

##### At least once

While used as a queue, a consumer needs to ack the message, say it gets failed in that, we get another delivery. So, a message is processed at least once. We can have different guarantees but they require different configurations.

##### High watermark consumption


## Cluster management
Leader-follower on the partitions.
Leader handles reads and writes. Followers work as backup. Controller chooses the leader replica such that the traffic is distributed evenly across cluster. Cluster also makes sure that of the leader does, follower can come up as the leader.


### Kafka as Stream vs kafka as queue
So Kafka have same working whether it is stream or a queue, the differentiation comes in the way it is used.

Kafka is a WAL storage, over which consumers commit the offsets. So, whether you want to use it as a  Q or a stream depends in how you are using it.


When we use kafka as a Q, we have one consumer group, and one consumer consumes from a partition. So, messages are processed in an order. Consumers have commits for their partitions. You generally do not reset the offset. Retention is also short, so these logs are buffers not assets. Scaling means adding more consumers uptil the number of partitions. Still kafka provides rebuilding capabilities by replaying the messages, no other Q system does this. ######Normal Queue vs Kafka

When we use kafka as streams, we generally have multiple consumer groups and retention is infinite, we can replay messages. 

Kafka is a string streaming solution, while a moderate Q as it does not provide DLQs out of the box.

### Working out kafka in an interview
- Message size can be anything in kafka, but for optimum performance max should be 1MB.
- One broker can store 1TB data and 1M reads per second.(depends on message size too)
- Scaling kafka, when needed. Scaling for a topic can happen by adding more partitions, this increases parallelism and hence the throughput. A partition is assigned to a key by hashing it if they key is not defined. This makes sure that the messages go equally to partititions.
	- Hot partition
		- default partitioning works well when ordering is not required
		- appending a random number like ts works but then if kafka is used as stream then it can complicate the aggregation logic
		- use a compound key
		- slow down producer by using backpressure, it can detect the consumer lag and try to slow down the production.

### Fault Tolerance and Durability in kafka
Multiple replicas are maintained on different brokers. Depending the the need, replication factor is chosen and thus we can impact the durability.
#### Consumer failure handling
- Consumers commit offsets to kafka, so even if they fail, whenever they come up they can resume.
- Also, think carefully when should a consumer commit during processing a message.

### Handling retries and errors
- Producer retries with idempotency
- Consumer retries are not there, we can have our own retry Q or DLQ.

### Performance Optimisations
- Compressing messages
- Batching messages
- Choosing the partition key well so message consumption can be parallelised.

### Retention Policies
- Check the storage capacity when longer retention is needed.



