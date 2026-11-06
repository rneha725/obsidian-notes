
### Fixed Window

**How it works:** one counter per aligned time bucket (`user:rule:minute`), using `INCR` plus `EXPIRE`, and the request is allowed if `count ≤ limit`.

Cheap but cannot handle bursty traffic.
**Pros**
- Simplest option: `INCR` is atomic on its own, so there's no Lua script and no race.
- Cheapest: one integer per key and one Redis round-trip.
- Exact for "per calendar period" semantics, so it's good for quotas and billing.

**Cons**
- **Boundary burst:** up to **2× the limit** across a reset (100 at :59 and 100 at :00).
- **Starvation, stated precisely:** a client that uses its full quota in the first second is locked out for the rest of the window. And on a _shared_ limit (per-IP, per-tenant, global), one noisy client can use the entire bucket early and starve everyone else until the reset.
- **Synchronized reset spikes:** all throttled clients retry at :00.

**Use when:** metering, daily or monthly quotas, and simple internal limits.

## Sliding Window Log
**How it works:** store the timestamp of every request (a Redis sorted set). On each request, drop entries older than `now − window`, count what remains, and allow if the count is under the limit.

Accurate but memory constraint for scale.
**Pros**
- **Exact:** "never more than N in any rolling window" is guaranteed.
- No boundary bursts and no approximation.

**Cons**
- **Memory is O(limit) per key.** A limit of 10k/min means up to 10k entries per user, and across millions of users that can overwhelm Redis.
- **Work per request:** the cost is trimming old entries plus a count. With a sorted set, `ZREMRANGEBYSCORE` is O(log n + removed) and `ZCARD` is O(1), so it's not a full scan, but it's still much heavier than an `INCR`.
- **Pitfall:** if you also log _rejected_ requests, an attacker who keeps hammering stays blocked forever and inflates memory. Only add the entry on allow.

**Use when:** small limits that need exact guarantees: OTP, login attempts, password resets.

## Sliding Window Counter

**How it works:** keep the previous and current window counts, and weight the **previous window's count** (not the limit) by how much of it still overlaps the rolling window.

```
elapsed_fraction = (now − current_window_start) / window_size
estimate = prev_count × (1 − elapsed_fraction) + curr_count
if estimate < limit → allow, curr_count++
```

No burst but approximation. Alternative to fixed window.

**Pros**
- No boundary burst, and smooth quota recovery.
- O(1) memory: two integers per key.

**Cons**
- **Approximation:** it assumes the previous window's traffic was evenly spread. It can slightly over-allow (if traffic was bunched late in the previous window) or over-reject (if bunched early).
- **Needs atomic read-compute-write** (Lua script), or concurrent requests race.
- **Harder to explain and audit:** "why was I blocked at 87?" Also, `Retry-After` is an estimate, not a fixed reset time.

**Use when:** general API load protection. It's the most common production choice.

## Token Bucket

**How it works:** a bucket holds up to **B** tokens (capacity) and refills at **R** tokens/sec. Each request takes one token, and if the bucket is empty the request is rejected. In practice you store `(tokens, last_refill_ts)` and refill **lazily** on each request: `tokens = min(B, tokens + (now − last_ts) × R)`.

Cheap and easy, handles real world traffic including bursty traffic, but cold start, tuning bucket size and refill rate are a problem.
**Pros**
- O(1) memory: two values per key.
- **Models real traffic:** it allows bursts up to B while enforcing an average of R. This is the only algorithm here where bursts are a deliberate feature.
- Two independent knobs: burst tolerance (B) and sustained rate (R).

**Cons**

- **Tuning B and R is the hard part:** B too big lets damaging bursts through, and B too small throttles legitimate spikes.
- **Full bucket at start (your "cold start" point):** a new or idle client starts with a full bucket and can immediately fire B requests. That's by design, but if B is large, a fresh client or a restarted limiter with reset state can hit the backend hard. Mitigations: start new buckets partially filled, or keep B small relative to what the backend can absorb.
- **No smooth ceiling over a window:** over any window T, a client can do up to `B + R×T`, so "max N per minute" isn't strictly guaranteed.
- **Needs atomic read-modify-write** (Lua), like the sliding counter.
- **Clock-sensitive:** refill depends on elapsed time, so use Redis server time, not node clocks.

**Use when:** public APIs where short bursts are acceptable (Stripe and AWS use this style), and limiting with burst allowance.

## One-line summary of each

- **Fixed window:** cheapest and simplest, but 2× boundary bursts and early-window starvation.
- **Sliding log:** exact, but memory is O(limit) and per-request work is heavier.
- **Sliding counter:** near-exact at O(1) memory, but it's an approximation and needs Lua.
- **Token bucket:** burst-friendly at O(1) memory, but needs B/R tuning and lets a full bucket burst from a fresh start.