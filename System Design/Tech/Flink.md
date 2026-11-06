### Overview
Flink is a stateful stream processor for unbounded data, designed to provide **exactly-once processing guarantees**, even in the face of failures, while maintaining high throughput and low latency.

Read: [[[TODO] Stream Processing vs Batch Processing]]

Take the example of counting the views every 5 minutes, say we are saying that we can maintain an in-memory cache to keep this counter
- But the app can crash and we will lose the data;
- Say an event comes later for a window;
- If we are adding a new pod for the app, what events should it get?
What about using cache for maintaining count for a window?
- Cache systems can crash and lose the data.
- Later events are possible here also.

### How Flink works?
- The fundamental units are: Nodes and Edges. Flink is a graph like dataflow system. 
	- Node: or operators perform computations, they are generally stateful.
	- Edges/Stream: move the data forward.
	- Source and sink are the edge nodes.

![[Pasted image 20261003123155.png|552]]

- While streams are responsible for passing on the data between nodes, nodes/operators can perform following operations on the received data:
	- Map: Transform each element individually
	- Filter: Remove elements that don't match a condition
	- Reduce: Combine elements within a key
	- Window: Group elements by time or count
	- Join: Combine elements from two streams
	- FlatMap: Transform each element into zero or more elements
	- Aggregate: Compute aggregates over windows or keys
	
	Example usage:
	
	```java
	DataStream<ClickEvent> clicks = // input stream
	clicks
	  .keyBy(event -> event.getAdId())
	  .window(TumblingEventTimeWindows.of(Time.minutes(5)))
	  .reduce((a, b) -> new ClickEvent(a.getAdId(), a.getCount() + b.getCount()))
	```

- It also emits updated state every time a state is updated.

##### Handling out of order events using 
It is a time config in the stream data that we get in flink, as in how much time to wait before we process the data for time window.

##### [Important choice] Window
Windows are based on time or count.
- Tumbling Windows: Fixed-size, non-overlapping windows
- Sliding Windows: Fixed-size, overlapping windows.
- Session Windows: Dynamic-size windows based on activity
- Global Windows: Custom windowing logic

Once window ends, flink emits output for that window.
Window choice can dramatically impact both the accuracy and performance of your streaming application. A tumbling window of 5 minute duration will emit once every 5 minutes. A sliding window of 5 minute duration with a 1 minute interval will emit every minute. It's worth reasoning backwards from the problem requirements to determine the least expensive window type that will give you the accuracy you need.


### Internals
#### Job and task managers
Job in Flink is basically the whole job graph, which defines the sink, source and the processing nodes in between. We will be using the connectors to create the sink and sources. These connectors also help us provide out of the box functionality to retry jobs by replaying in case of failures. 

- Job Manager: It is the coordinator node, it schedules tasks, coordinate checkpoints and handles failures. This is the supervisor.
- Task Manager: Manages the task on the node and communicates the status to job manager. Each task manager have task slots, where we can schedule the task execution. Each has the resources so we can run different tasks in parallel. We can isolate or share resources/memory between tasks using task slots.

To ensure high availability, we have replicas of job managers which are leader based cluster.

#### State management
In case of failures, we need to come back to the state that we were in. 

Storage: You can store the state, in memory, in node disk, or RocksDB according to the use case. Performance degrade and capacity and durability increase as we move up in the options.

##### Checkpointing
To recover from failures, Flink stores states/snapshots of the system. This is done using checkpointing. 
Steps of checkpointing (variation of Chandy Lamport Algo):
1. Job manager asks source to start checkpointing. Sends barrier event.
2. Source stores this as the checkpoint state.
3. Operators wait for all the inputs to provide the checkpoint event, they use buffer for newer events, not needed for checkpoint.
4. Once they receive all, they create the snapshot and push the checkpoint event to next operator.
5. Job manager waits for all the nodes including sink to ack the checkpoint.

Recovery Process:
1. Restart the job (for any task failure)
2. Restore every node to last checkpoint
3. Rewind the source(s) on the last checkpoint
4. Replay the events from the source

##### Exactly once processing
- Have an idempotent sink
- Have a transactional sink, which commits the checkpoint once it receives the barrier.

In case of failure, we restore the state from these check points.

Once a failure is detected in the system by the job manager, it tried to recover. It might reassign some tasks. When a node fails, it is necessary to restart the job and not only the task to maintain exactly-once guarantees. It uses Chandy-Lamport algo for exactly-once guarantees.
To restart, we restore the check point and replay the source events from the checkpoint.

> Flink guarantees exactly-once semantics for internal state operations, but this doesn't automatically extend to external systems. For example, when making API calls or writing to external databases, you may still process the same record multiple times in case of failure and recovery. You need to implement idempotent operations or transactional behaviour when interacting with external systems to achieve true end-to-end exactly-once processing.


### Flink usage by an example
Situation:
We want to count number of views in last 5 minutes window.


### Flink Usages
In an interview make sure the discuss the Flink state management, window size, consider if you need exactly-once processing. Also, Flink's add a lot of operational complexity, and state management is one of the most difficult task for if you have it.
#### When not to use
- Not all stream processing requires Flink, say we only want to transform the data, then kafka and a consumer is enough.

#### When to use