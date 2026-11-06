# LLD / Design-a-Class Coding Questions

**Who asks this (2025–2026):** Google, Databricks, LinkedIn, Apple, Airbnb, Salesforce, Twilio, and Stripe (as multi-part builds).

**How these rounds work:** you write one or more classes with clean methods. Then the interviewer adds a new requirement. Code so the next requirement is easy to add.

**Do every problem in Java, with a few tests.** (P) = needs LeetCode Premium.

## How to run an LLD round

1. **Clarify:** list the operations and their inputs and outputs. Ask about scale and concurrency.
2. **Entities:** name the classes and what each one owns.
3. **Interface first:** write the method signatures before the bodies.
4. **Simplest working version:** get it correct before making it clever.
5. **Tests:** write 3–4 small tests, including one edge case.
6. **Extend:** expect "now make it thread-safe" or "now add X". Keep rules behind an interface so a new rule is a new class.

**Patterns that come up most:** Strategy (swap algorithms), Observer (pub/sub), State (order or payment status), Factory (create the right type).

## Stage 1: one class, one data structure

| Problem                          | Hint                          | Status | My Notes |
| -------------------------------- | ----------------------------- | ------ | -------- |
| [155 Min Stack                   ](https://leetcode.com/problems/min-stack/) | store extra state per element |        |          |
| [359 Logger Rate Limiter (P)     ](https://leetcode.com/problems/logger-rate-limiter-p/) | asked at Google               |        |          |
| [362 Design Hit Counter (P)      ](https://leetcode.com/problems/design-hit-counter-p/) | asked at Databricks           |        |          |
| [380 Insert Delete GetRandom O(1)](https://leetcode.com/problems/insert-delete-getrandom-o1/) | map + array together          |        |          |
| [1472 Design Browser History     ](https://leetcode.com/problems/design-browser-history/) | list + current pointer        |        |          |

## Stage 2: caches and key-value stores

| Problem | Hint | Status | My Notes |
|---|---|---|---|
| [146 LRU Cache](https://leetcode.com/problems/lru-cache/) | asked at Twilio |  |  |
| [981 Time Based Key-Value Store](https://leetcode.com/problems/time-based-key-value-store/) | asked at Apple |  |  |
| [1146 Snapshot Array](https://leetcode.com/problems/snapshot-array/) | versioned store |  |  |
| [1797 Design Authentication Manager](https://leetcode.com/problems/design-authentication-manager/) | expiring tokens |  |  |
| [460 LFU Cache](https://leetcode.com/problems/lfu-cache/) | harder eviction rule |  |  |

## Stage 3: iterators and parsers

| Problem | Hint | Status | My Notes |
|---|---|---|---|
| [284 Peeking Iterator](https://leetcode.com/problems/peeking-iterator/) |  |  |  |
| [341 Flatten Nested List Iterator](https://leetcode.com/problems/flatten-nested-list-iterator/) |  |  |  |
| [227 Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/) | Apple asked a calculator-style parser |  |  |
| [224 Basic Calculator](https://leetcode.com/problems/basic-calculator/) | adds brackets |  |  |

## Stage 4: multi-class designs (build them yourself)

| Problem | Hint | Status | My Notes |
|---|---|---|---|
| Rate limiter | fixed window, sliding log and token bucket behind one interface. Asked at LinkedIn and Apple.<br>Follow-up: make it thread-safe. Then: limit per user and per API. |  |  |
| Digital wallet | accounts, transfers, history, refunds. Asked at Airbnb.<br>Your ledger background fits this one well. Show double-entry style records and idempotent transfers. |  |  |
| Stock trading app / order book | place, match and cancel orders. Asked at Salesforce. |  |  |
| HTTP router | match paths with parameters and wildcards. Asked at Google.<br>Hint: a trie of path segments. |  |  |
| Multi-part data processor (Stripe style) | read CSV or JSON, validate it, then report on it.<br>Build it in 3–4 stages, each adding a rule. Don't jump ahead. |  |  |
| Key-value store with transactions | `set`, `get`, `begin`, `commit`, `rollback`, with nested transactions. |  |  |
| Topic store / pub-sub | publish to a topic, get the latest value, subscribe to updates.<br>A DocuSign interviewer expected a pub/sub-style answer to a "latest value by topic" question. |  |  |
| Job scheduler | schedule a job at a time or on repeat; cancel it; list upcoming jobs. |  |  |

## Stage 5: hard

| Problem | Hint | Status | My Notes |
|---|---|---|---|
| [355 Design Twitter](https://leetcode.com/problems/design-twitter/) |  |  |  |
| [588 Design In-Memory File System](https://leetcode.com/problems/design-in-memory-file-system/) (P) |  |  |  |
| [432 All O(1) Data Structure](https://leetcode.com/problems/all-oone-data-structure/) |  |  |  |

## You've mastered it when

- You can build a Stage 4 design in 45 minutes with tests.
- You can add a new requirement without rewriting existing classes.
