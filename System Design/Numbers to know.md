### Cheatsheet

| Component          | Key Metrics                                                                                          | Scale Triggers                                                                                                            |
| ------------------ | ---------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| **Caching**        | - ~1 millisecond latency  <br>- 100k+ operations/second  <br>- Memory-bound (up to 1TB)              | - Hit rate < 80%  <br>- Latency > 1ms  <br>- Memory usage > 80%  <br>- Cache churn/thrashing                              |
| **Databases**      | - Up to 50k transactions/second  <br>- Sub-5ms read latency (cached)  <br>- 64 TiB+ storage capacity | - Write throughput > 10,000 Transactions per second<br>- Read latency > 5ms uncached  <br>- Geographic distribution needs |
| **App Servers**    | - 100k+ concurrent connections  <br>- 8-64 cores @ 2-4 GHz  <br>- 64-512GB RAM standard, up to 2TB   | - CPU > 70% utilization  <br>- Response latency > SLA  <br>- Connections near 100k/instance  <br>- Memory > 80%           |
| **Message Queues** | - Up to 1 million msgs/sec per broker  <br>- Sub-5ms end-to-end latency  <br>- Up to 50TB storage    | - Throughput near 800k msgs/sec  <br>- Partition count ~200k per cluster  <br>- Growing consumer lag                      |

### Availability Numbers
| Availability | Per year     | Per month  | Per day     |     |
| ------------ | ------------ | ---------- | ----------- | --- |
| 99% (two 9s) | 4 days       | 7 hours    | 15 min      |     |
| **99.9%**    | **10 hours** | **45 min** | **1.5 min** |     |
| 99.99%       | ~1 hour      | ~4.5 min   | ~9 s        |     |
| 99.999%      | ~5 min       | ~26 s      | <1 s        |     |
use x10 for each, remember for 99.9%.

### Redis: Numbers to know
| Metric                                           | Typical                                                                                                         |
| ------------------------------------------------ | --------------------------------------------------------------------------------------------------------------- |
| Ops/sec per instance (simple `GET`/`SET`/`INCR`) | **~100k** without pipelining; up to ~1M with pipelining                                                         |
| Latency within the same data center              | **Under 1 ms** (~0.2 to 0.5 ms network round-trip plus microseconds of execution)                               |
| Lua script ops/sec                               | Lower than a plain `INCR`; roughly **30k to 80k** for small rate-limit scripts                                  |
| Command execution model                          | **Single-threaded** (I/O threads since Redis 6 help with networking, but command execution is still one thread) |
| Practical memory per instance                    | **25 to 50 GB** (larger instances make fork-based persistence and failover slow)                                |


