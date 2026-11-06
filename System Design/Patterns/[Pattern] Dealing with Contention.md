## Problem
When multiple requests are fighting for same resource. You need to handle it well otherwise we will end up with inconsistent weird state. Situations where two people came for a movie and ended up having same seats, now they are fighting over who will sit on whose lap. Funny but we will still try to avoid these situations. 
This is but different from other scale problems we generally deal with load, that load is not necessarily fighting for common resources, here even on low scale we have this problem.

For a non-contention aware query, we can do a read-modify-write but in case of two requests running the same query, we will end up in the inconsistent state.
Note: for this pattern to work, we make sure that we are dealing with single home of the data and it resides in single db.

## Solutions
### Atomic Update
Get a CAS query on the contended resource and you are all set.
```sql
UPDATE booking_table
SET status = 'TAKEN'
WHERE seat_number = '123' 
	AND status = 'PENDING'
	AND event_id = '345';
```
- *Tradeoffs*: Simple and no locks needed, but it cannot handle complex queries. 
	- Say we want to update 'multiple rows or nothing' type of writes. Can be done though with the number of rows returned, checking the number and then update. Still complicated.

### Pessimistic Locking
for read-decide-write.
It is pessimistic, because this one believes that conflicts will happen a lot. So lock them once you start reading the entries for updating. 
```sql
BEGIN;

-- 1. Lock the 4 chosen seats (fixed order avoids deadlocks)
SELECT seat_number, status
FROM seats
WHERE concert_id = 'weeknd_tour'
  AND seat_number IN ('A15', 'A16', 'A17', 'A18')
ORDER BY seat_number
FOR UPDATE NOWAIT; -- optional NOWAIT

-- 2. App checks: did we get 4 rows, all 'available'?
--      No  → ROLLBACK; tell user to pick other seats
--      Yes → continue

-- 3. Claim the seats
UPDATE seats
SET status = 'sold', user_id = 'user123'
WHERE concert_id = 'weeknd_tour'
  AND seat_number IN ('A15', 'A16', 'A17', 'A18');

COMMIT;
```
- 'FOR UPDATE' waits for the lock by default, 'NOWAIT' fails the transaction if say partial lock is held by other.

- ***Issues***:
	- Locking for long, other transactions can pile up of fail.
	- Ordering while locking, say two transactions lock same resources but in different order, then there is a *deadlock*.

##### How ordering prevents deadlock?
- Transaction A: 1,2,3,4
- Transaction B: 3,4,5,6

- Say B gets the lock on 3, now A waits.
- Say A gets the lock on 3 first, B waits.

Assume now we have no ordering.
- Transaction A: 1,2,3,4
- Transaction B: 3,2,4,5

- B gets lock on 3 and waits for 2, A gets lock till 2, and waits for 3.
  
- Say B was ordered: 
- Transaction A: 1,2,3,4
- Transaction B: 2,3,4,5
- B got the lock on 2, not A will wait.

This ordering is also on the table orders.

**Ordering works**: when you lock all the resources upfront in one order.
**Ordering breaks**: 
- When you acquire a lower order resource later in the transaction after acquiring first set of locks.
- When table ordering is also broken.
- Other side effect: foreign key might lock the parent row, some trigger,

**Takeaway**: ordering cuts deadlocks a lot, but doesn’t remove them all. Always add **retry on deadlock error** as a safety net.

### Optimistic Locking
We are hopeful that the conflicts are rare.

We always check a column, which is **always increasing** and every write updates it, like a timestamp (assuming clocks are such that no ts can collide), or an extra version field. Before inserting we check if the version number is what we read.
If the column is not ever increasing, we might face ABA problem.

-----
Above solutions were about contending for same row, there are scenarios where two transaction rows are overlapping.

------
### Write Skew
[[Write Skew]]

### Distributed Locks
What if you want to lock a seat and proceed for the payment, although we have a transaction locking the row for 10 minutes, payment completed notification can go to any server, assuming we have stateless servers, What about the lock then?

Note that these observations are based on, once you get a lock, you can execute the transaction. That lock can belong to the row which is getting written by a lot requests, or a parent lock which let some transaction execute.

```json
Start
Get lock -> anywhere

-- do whatever in any table

Commit
```

Here we use distributed locks, we lease the lock as data somewhere:
- **Redis with TTL**: [[Redis#2. Redis as a distributed lock|as distributed lock]], we will face the hiccup when one process with the lock goes idle, past its TTL and wakes up later when another process already took the lock. Advantage is speed and simplicity. SPOF. Use this when you are holding lock to reduce traffic on the db, and it is okay if there are some failures in locking.
- **Db Column**: We can have another two db column for the ts when the lock will be released and the id which holds it. *Tradeoff*: We can have better guarantee than redis, as writes can check for the owner lock at the time of writing, but we will have more slower writes now. Also, these columns can be contended under heavy load. Use this when you are not okay with lock logic failures.
- **Zookeeper**: Most robust and takes away the cons of the above. But adds operational complexity and infra cost.

***Note***: Try to keep contended resources in same db otherwise we will fall into distributed transaction. In case we have leaderless cluster, we can try LWW, vertor dbs or CRDTs.

#### Your check is a predicate on the row you're writing
```json
read a row to get some data

condition on that data: update another row

Fix: UPDATE stock SET qty = qty - 1 WHERE id = 7 AND qty > 0;
```

Use a conditional UPDATE and gate any follow-up work on the affected row count, whether that's a counter decrement, a status flip, or claiming a row. No explicit lock needed since the write is atomic on its own. It's the simplest thing that works, so reach for it first.

#### You have to read, decide in application code, then write
The check isn't something a WHERE clause can express, like finding an open block of seats or checking a value on another row. Pessimistic locking with FOR UPDATE holds the rows you read across the gap, and it's predictable and easy to reason about. A good default when contention is high.

```sql
BEGIN;
SELECT * FROM seats WHERE row = 'A' FOR UPDATE;   -- lock what you read
-- app code finds A5–A8 free
UPDATE seats SET status = 'held', held_by = 'neha'
WHERE id IN ('A5','A6','A7','A8');
COMMIT;                                            -- locks released
```

#### Same read-decide-write, but collisions are rare
Optimistic concurrency. Tag the row with a value that changes on every write, a version counter, a timestamp, or a business value that only moves one way, and retry when your write finds it already moved. Better performance and no blocking when conflicts are infrequent.

```json
1. Read:    SELECT qty, version FROM stock WHERE id = 7;
            → qty = 5, version = 12

2. Decide:  app code does its logic, decides new qty = 4

3. Write:   UPDATE stock SET qty = 4, version = 13
            WHERE id = 7 AND version = 12;
```

#### Your invariant spans rows that never collide
This is write skew, and the simpler tools miss it because there's no shared row to lock or guard. Reach for SERIALIZABLE isolation so the database catches the conflict, or materialize the invariant onto a single row you can lock.

#### The hold has to outlive a single transaction
A distributed lock, for when exclusivity has to span a wait, a call to another service, or several steps, like reserving a seat during checkout. It's also the move for user-facing competitive flows, where a short reservation is as much about UX as correctness.
Use Redis or db row lock

### Interview Notes
- Call it out during interviews, maybe during NFRs while discussing the consistency.

### Examples:
- **Online Auction Systems for bidding**. Use Optimistic.
- **Ticketmaster/Event Booking**: We can think of pessimistic in this case, but see that we are waiting for payment, so we cannot do this in one transaction, we do this by temporary booking the seat, say for 10 minutes.
- **Banking/Payment Systems**: Use Pessimistic or optimistic.
- **Ride Sharing Dispatch**:Temporary status reservations shine here. You can set driver status to "pending_request" when sending ride requests, which prevents multiple simultaneous requests to the same driver. Use either a cache with TTL for automatic cleanup when drivers don't respond within 10 seconds, or a status column with an expiration timestamp, where a request that's gone stale reads as free without any cleanup job.
- **Flash Sale**: An update type of write can work, and it will be very simple, but say the hit rate is high 1M/sec, then use Redis counter so db is saved with so many queries. If traffic is high, then OCC and pessimistic both are not good idea. Also, if the whole thing requires you to pay, it will be better to use a temporary reservation.
- **Review System**: For updating rating. We can use optimistic locking as the conflicts are rare. Also, if we will just store sum and number of reviews that will mean we need only update query.

##### When not to use
- Low contention
- Read heavy
- Single user ops

### Deep Dives
- Deadlock prevention in pessimistic locking
	- Always order using a key.
	- In any case, we have a deaddlock prevention in dbs. => aborts when realises.
- When everyone needs one resource:
	- We try to distribute it, see if the system is eventually consistent.
	- We can queue the requests => worker can die. Again eventually consistent.

### Quiz
#### Contention Strategies: Q&A

##### Quick Ladder

1. **Check is on the row you write** → Conditional UPDATE
2. **Read-decide-write, high contention** → Pessimistic locking (`FOR UPDATE`)
3. **Read-decide-write, rare conflicts** → Optimistic concurrency (version column)
4. **Rule spans rows with no shared row (write skew)** → SERIALIZABLE, or materialize a guard row
5. **Hold must last across steps or time** → Lease / distributed lock with TTL, plus a final conditional UPDATE

---

##### Q1. Bank withdrawal: balance must never go below zero

**Question:** A user withdraws ₹500. Which strategy is the best first choice?

- A. Optimistic locking with a version column
- B. Conditional UPDATE: `SET balance = balance - 500 WHERE id = X AND balance >= 500`
- C. Redis distributed lock on the account
- D. SERIALIZABLE isolation

**Answer:** B

**Why:** The rule is on the same row being changed. Put it in the WHERE clause. The DB checks and writes in one atomic step. 1 row updated = success, 0 = insufficient funds.

---

##### Q2. Cron job on 10 servers

**Question:** Ten identical servers run the same cron job. You want the nightly report generated once. An occasional duplicate is harmless. What fits best?

- A. `SELECT ... FOR UPDATE` on a reports table
- B. Optimistic version check
- C. Redis lock with a TTL (`SET key NX PX`)
- D. SERIALIZABLE isolation

**Answer:** C

**Why:** You're guarding an action, not a DB row. It's an efficiency lock, so a rare duplicate just wastes work. Redis is fast, and the TTL frees the lock if a server dies.

---

##### Q3. Wiki page editing

**Question:** Two people editing the same page at once is rare. The app reads the page, the user edits, then saves. Best strategy?

- A. Optimistic concurrency with a version column; retry or show a conflict on mismatch
- B. `FOR UPDATE` held while the user edits
- C. SERIALIZABLE isolation on every save
- D. A guard row per wiki

**Answer:** A

**Why:** Conflicts are rare, so optimistic wins. No blocking, and the rare clash is caught at save time (version changed → 0 rows updated). Holding `FOR UPDATE` during editing would block others for minutes.

---

##### Q4. Meeting room booking

**Question:** Insert a booking only if no existing booking overlaps 3–4 PM. Two users check at once, both see none, both insert. What fixes this?

- A. Optimistic version on the bookings rows
- B. `FOR UPDATE` on the overlapping bookings
- C. Lock the room row with `FOR UPDATE` before checking (or use SERIALIZABLE)
- D. Conditional UPDATE on the new booking

**Answer:** C

**Why:** The check found zero rows, so there's nothing to lock or version. This is write skew with phantoms. Lock the room row so both users touch the same row, or let SERIALIZABLE detect the conflict and abort one.

---

##### Q5. 4 adjacent seats at a popular concert

**Question:** The app must scan row A to find 4 free adjacent seats. Many users target the same rows. Best strategy?

- A. Optimistic version check per seat
- B. `SELECT ... FROM seats WHERE row = 'A' FOR UPDATE`, then pick and write
- C. Single conditional UPDATE
- D. Redis lock held for 10 minutes

**Answer:** B

**Why:** Finding an adjacent block needs app logic, so it's read-decide-write. Contention is high, so pessimistic beats optimistic (which would cause a retry storm). Keep the transaction short.

---

##### Q6. Hotel room held during checkout

**Question:** Hold a room for 15 minutes while the user enters payment. Requests may land on any server. What do you use?

- A. A long-running transaction with `FOR UPDATE`
- B. Optimistic version check at payment time only
- C. SERIALIZABLE isolation
- D. A reservation with expiry (`held_by`, `held_until` or a Redis TTL key), plus a final conditional UPDATE at payment

**Answer:** D

**Why:** The hold must outlive a single transaction and be visible to every server. Store it as data with an expiry. At payment, confirm with `UPDATE ... WHERE held_by = user AND held_until > now()`, so an expired hold can't sell.

---

##### Q7. Flash sale: what's the WORST choice?

**Question:** 1,000 units, a million users clicking "buy" within seconds. Which approach is worst?

- A. Conditional decrement: `WHERE qty > 0`
- B. Redis atomic decrement as a gatekeeper, DB as final guard
- C. Optimistic concurrency with a version column
- D. Splitting stock into several bucket rows

**Answer:** C

**Why:** Under extreme contention, almost everyone fails and retries, again and again. That retry storm hammers the DB. Conditional decrement gives an instant yes/no with no retries.

---

##### Q8. Doctors on call

**Question:** At least one doctor must be on call per shift. Alice and Bob each set only their own row to off-call at the same time. Correct fix?

- A. Optimistic version on each doctor's row
- B. Conditional UPDATE on the doctor's own row
- C. Keep `on_call_count` on the shift row and do `UPDATE shifts ... WHERE on_call_count > 1`
- D. Read committed isolation

**Answer:** C

**Why:** The rule spans rows that never collide, so per-row checks miss it. Moving the rule onto one shift row creates the collision and turns it into a simple conditional UPDATE. SERIALIZABLE would also work.

---

##### Q9. Workers claiming jobs

**Question:** Each job must be processed by exactly one worker. Simplest correct way to claim a job?

- A. `UPDATE jobs SET owner = 'w3', status = 'running' WHERE id = 99 AND status = 'pending'`, then check row count
- B. SERIALIZABLE isolation
- C. Redis lock per job
- D. Read the job, then update it without a condition

**Answer:** A

**Why:** The check (still pending) is on the row being written. The conditional UPDATE claims it atomically. 1 row = you own it, 0 = someone else got it. (`FOR UPDATE SKIP LOCKED` is a common variation for picking the next job.)

---

##### Q10. Business average rating

**Question:** Users submit reviews; the business rating must stay accurate. Simplest race-free design?

- A. Read `avg_rating`, compute the new average in code, write it back with no check
- B. Store `rating_sum` and `num_ratings`; do `SET rating_sum = rating_sum + r, num_ratings = num_ratings + 1`
- C. Redis lock per business held during the review
- D. `FOR UPDATE` on the reviews table

**Answer:** B

**Why:** Atomic increments need no read, so there's no gap and no race. Compute the average at read time. Optimistic with a version column also works (conflicts are rare), but this is simpler. Option A loses updates.

---

##### Q11. Redis lock and a GC pause

**Question:** A server pauses for 30s, its Redis lock expires, another server takes the lock, then the first wakes up and writes. What prevents damage?

- A. A longer TTL
- B. Fencing tokens: an increasing number per lock; storage rejects writes with an older token
- C. Retrying the write
- D. Using a faster Redis

**Answer:** B

**Why:** A longer TTL only shrinks the window. Fencing tokens let the storage reject the stale write, because the new holder's token is higher. This makes a lock safe for correctness.

---

##### Q12. Ride app driver offer

**Question:** A driver is offered to one rider for 15 seconds. If not accepted, the driver goes to the next rider. Best fit?

- A. `FOR UPDATE` on the driver row for 15 seconds
- B. Optimistic version on the driver row
- C. A short lease with TTL on the driver (Redis key or `offered_until` column)
- D. SERIALIZABLE isolation

**Answer:** C

**Why:** The hold spans a wait for a human, so it must outlive a transaction. A lease with TTL auto-expires, letting the driver move on. Acceptance is confirmed with a conditional UPDATE.

---

##### Key Ideas to Remember

- **Conditional UPDATE first.** If the check is on the row you write, put it in the WHERE clause and check the row count.
- **High contention → pessimistic. Rare conflicts → optimistic.**
- **Write skew** happens when transactions write different rows. Fix it with SERIALIZABLE or a single guard row.
- **You can't lock rows that don't exist.** Lock a parent row instead.
- **Long holds = data with an expiry**, not open transactions.
- **Redis locks are for efficiency.** For correctness, add fencing tokens or let the DB be the final guard.