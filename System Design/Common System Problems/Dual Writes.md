[A good article](https://dev.to/ankitdevcode/the-outbox-pattern-solving-the-dual-write-problem-in-distributed-systems-392d)

Dual write problem is when you are trying to write to multiple systems at a time. A very common example is writing to a db and then to a queue. You might want to do something like this:

```json
START TRANSACTION
	write-to-db           //1. fails here, transaction fails, we can retry
	produce-to-kafka      //2. if we fail here, we can skip committing the transaction
	commit                //3. If it fails here, while do would not have the write but q will have the message, a retry will mean another publish to the queue
END TRANSACTION
```

### Solutions to Dual write problem

#### 1. Transactional outbox pattern
[[[Pattern] Outbox Pattern]]

- That we might need an idempotent consumer here, which is easy to implement.
#### 2. Event Sourcing

