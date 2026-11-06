# Concurrency Coding Questions (Java)

**Who asks this (2025–2026):**

- **LinkedIn:** a concurrency follow-up on the phone screen, and a dedicated concurrency interview in some loops.
- **Databricks:** a concurrency round.
- **Apple:** backend and platform teams.
- **Stripe:** race conditions show up in the bug-squash round.
- **MongoDB** (outside your top 12): a full concurrency round. Good practice material.

(P) = needs LeetCode Premium.

## Java tools to know cold

| Tool | Use it for |
|---|---|
| `synchronized` | simple mutual exclusion |
| `ReentrantLock` + `Condition` | timed or interruptible locking, several wait queues |
| `ReadWriteLock` | many readers, one writer |
| `Semaphore` | limit how many threads enter |
| `CountDownLatch` | wait until N things finish |
| `ConcurrentHashMap` | thread-safe map; use `computeIfAbsent` / `merge` |
| `AtomicLong` / `LongAdder` | lock-free counters |
| `ExecutorService` | thread pools |
| `CompletableFuture` | async calls, fan-out and fan-in |
| `volatile` | visibility of a flag across threads |

## Stage 1: LeetCode warm-ups

| Problem | Hint | Status | My Notes |
|---|---|---|---|
| [1114 Print in Order](https://leetcode.com/problems/print-in-order/) | wait for another thread |  |  |
| [1115 Print FooBar Alternately](https://leetcode.com/problems/print-foobar-alternately/) | two threads take turns |  |  |
| [1116 Print Zero Even Odd](https://leetcode.com/problems/print-zero-even-odd/) | three threads take turns |  |  |
| [1195 Fizz Buzz Multithreaded](https://leetcode.com/problems/fizz-buzz-multithreaded/) | four threads, one counter |  |  |
| [1279 Traffic Light Controlled Intersection](https://leetcode.com/problems/traffic-light-controlled-intersection/) (P) | one lock, simple state |  |  |

## Stage 2: core building blocks

| Problem | Hint | Status | My Notes |
|---|---|---|---|
| [1188 Design Bounded Blocking Queue](https://leetcode.com/problems/design-bounded-blocking-queue/) (P) | build it twice, with `wait/notify` and with `ReentrantLock` + `Condition` |  |  |
| [1117 Building H2O](https://leetcode.com/problems/building-h2o/) | group threads in sets |  |  |
| [1226 The Dining Philosophers](https://leetcode.com/problems/the-dining-philosophers/) | avoid deadlock |  |  |
| Read-write lock from scratch | many readers or one writer; then make writers not starve |  |  |

## Stage 3: thread-safe components

| Problem | Hint | Status | My Notes |
|---|---|---|---|
| Thread-safe LRU cache | one lock first, then discuss lock striping |  |  |
| Thread-safe rate limiter | token bucket using `AtomicLong` |  |  |
| Thread-safe hit counter | many writers, one reader |  |  |
| Expiring / delayed queue | items become available after a delay |  |  |
| Bank transfer between accounts | avoid deadlock by always locking accounts in ID order. Ties to your ledger work. |  |  |

## Stage 4: hard

| Problem | Hint | Status | My Notes |
|---|---|---|---|
| [1242 Web Crawler Multithreaded](https://leetcode.com/problems/web-crawler-multithreaded/) (P) | single-threaded first, then speed it up |  |  |
| Job scheduler | a worker pool that runs jobs at a set time, including repeating jobs |  |  |
| Your own thread pool | fixed workers, a task queue, graceful shutdown |  |  |
| Bug drills (Stripe style) | fix a balance check that isn't atomic, then add idempotency so a retry doesn't charge twice |  |  |

## Practice exercises (small, hands-on)

Do each one in a scratch Java project. Run it and watch the bug happen before you fix it.

1. **See a race:** two threads each add 1 to a counter a million times. Note the wrong total. Fix it three ways: `synchronized`, `AtomicInteger`, `LongAdder`.
2. **Make a deadlock:** two threads take two locks in opposite order. Take a thread dump (`jstack`). Fix it with lock ordering, then with `tryLock` and a timeout.
3. **Visibility bug:** a worker loops on a `boolean running` flag that another thread sets to false. See it hang. Fix it with `volatile`.
4. **Build a CountDownLatch** using `wait/notify`.
5. **Build a Semaphore** using `ReentrantLock` + `Condition`.
6. **Producer-consumer:** two producers, three consumers, a bounded queue. Shut down cleanly with a "poison pill" item.
7. **Check-then-act bug:** `if (!map.containsKey(k)) map.put(k, v)` on a `ConcurrentHashMap` from many threads. Show the bug, then fix it with `computeIfAbsent`.
8. **Fan-out with CompletableFuture:** call 5 fake services in parallel with a timeout. Combine the results. Handle one service failing.
9. **Clean executor shutdown:** `shutdown`, `awaitTermination`, then `shutdownNow`. Handle `InterruptedException` correctly.
10. **Idempotent payment handler:** two threads send the same request ID at the same time. Make sure it charges only once.

## Questions to answer out loud

- `synchronized` vs `ReentrantLock`: when do you need the lock?
- What does "happens-before" mean? What does `volatile` guarantee, and what doesn't it?
- How does `ConcurrentHashMap` stay thread-safe without one big lock?
- What are the `ThreadPoolExecutor` settings and rejection policies?
- What 4 conditions cause a deadlock? How do you prevent each one?
- Optimistic vs pessimistic locking: when would you pick each?
- How do you size a thread pool for CPU-bound vs IO-bound work?
- When do Java 21 virtual threads help, and when don't they?

## You've mastered it when

- You can write a bounded blocking queue from memory in 15 minutes.
- You can explain why your code is safe, not just that it passes.
