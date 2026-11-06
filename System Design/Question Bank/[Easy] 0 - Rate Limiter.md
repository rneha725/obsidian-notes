[Hello Interview - Rate Limiter](https://www.hellointerview.com/learn/system-design/problem-breakdowns/distributed-rate-limiter)

### Gaps that I can see in me
---
- How would rules work? 
	- so for every api, for every user, I might have some rules, like 10 minutes, max 30 requests, so when would we start creating this dynamic implementation of the rules?
	- Answer: We will have a rule storage in a db(as mostly static, we can store these in-memory and fetch updates maybe using a consumer or poll according to the use case), we will have a Redis store the data for the rate limits. In the API, first we will check the rate limit, which will check Redis, on failure it will start creating the data in Redis using the rules in DB.
- Where to keep our Ratelimiter:
	- In the application -> global rate limits cannot be applied, LB should be stateful for this to work, an app failure might reset the Rate limiter data, good when rate limit rules are not that hard. We need it for sanity.
	- Another service which can be called by other services: Another SOF, Network hop, Operational complexity. Goods: We can apply very deep specific rules to the rate limiter. For example, premium users should get 10x limit. In case rate limiter is slow or fails, we do not know what is the right call? to open the circuit or to close? Slow rate limiter response.
	- In the API gateway: A basic level of hard rate limiting, services do not need to have a rate limiter. Bads: Basic rate limiting based on the headers, request parameters etc. We need to keep is fast.
- Consider different types of rate limit you will be applying, consider on what attribute and why? and types: global, per user, per api, per ip etc

### Deep Dives
- Scale of 1m per second:
	- Multiple Redis instances, as we are having Lua script here, one instance can handle 100k per second ops => 10 instances at least.
	- Each user_api key having their own bucket and say we are using [[Rate Limit Algorithms#Token Bucket|token bucket]] algorithm.
- High availability and fault tolerance:
	- [[Redis]] has master slave replicas within cluster.
	- For fault tolerance, define which strategy is best for your use case, fail-open or fail-close.
- Minimize latency:
	- Network hop for Redis, and network call can take anytime.
	- We can use connection pooling so we do not need to have TCP handshake again and again(max 50ms)
	- We can try to connect to geographically nearer server.
	- Other solutions but do not dwell: Request batching, local cache(issue with the stale entries), However, these optimizations add complexity.
- Hot keys:
	- Client side ratelimiting
	- Request batching
	- Have DDoS protection layer
- Updating Rules:
	- Pull based: having a poller, easy and simple, but updates won't be real time.
	- Push Based: Like [[Zookeeper]], but adds a lot of complexity, only use when RL is the critical path.

### Rate limiting algorithms
...and choosing one according to your needs.

Read [[Rate Limit Algorithms]]:
- **Fixed window:** cheapest and simplest, but 2× boundary bursts and early-window starvation.
- **Sliding log:** exact, but memory is O(limit) and per-request work is heavier.
- **Sliding counter:** near-exact at O(1) memory, but it's an approximation and needs Lua.
- **Token bucket:** burst-friendly at O(1) memory, but needs B/R tuning and lets a full bucket burst from a fresh start.
