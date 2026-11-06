## Ways to scale reads
- [[#Optimize Database]]
	- Hardware Upgrade
	- Database Indexing
	- De-normalizing data
	- Use materialized view
- [[#Scale database horizontally]]
	- Read Replicas
	- Sharding -> mostly a write scaling solution, better to go for caching
- [[#Add Caching]]
	- Application Level caching

### Optimize Database
#### Indexing
- [[Database Indexing]]
- It is okay to add more index and the write performance is not affected much. It is generally shown as a tradeoff, but not a real one.

#### Hardware Upgrade
- Increase RAM.
- Moving from HDDs to SSDs directly reduces random-read latency, often improving random I/O by 10–100x.

#### De-normalizing data
- It complicates writes but simplifies reads, in case the reads a lot more and writes are moderate we can try this.

#### Use materialized view
- Precompute the aggregation, frequent writes won't be good.

### Scale database horizontally
Rough estimates: 100k requests per second when you need more servers

#### Read Replicas
- Add replicas and write will go to the primary, and reads will go to the replicas.
- *Tradeoffs*: Eventual consistency, or according to the replica lag, write throughout may decrease. But we also get redundancy for availability of the system.

#### Sharding
- If the dataset size is large enough, we need to shard the data.
- **Functional Sharding**: Have different database according to the function: user, product etc.
- **Geographic sharding**: for global read scaling.
- **Records Sharding**: Same data but on different machines in case dataset is huge.
- *Tradeoffs*:
	- Need to make sure that the operations are using minimal different shards. 
	- Adds more operational cost. It it mostly a [[[Pattern] Scaling Writes|write scaling solution]].

### Add Caching
Very useful when the situation is: data that rarely changes between requests.
Cache serves from memory -> faster than a db.

#### Application Level caching
 We use cache like mem-cached or [[Redis]] for this. Cache sites between application and db. General problem here is cache invalidation:
 [[Cache Problems#Cache Invalidation|Cache Invalidation Strategies]]

#### CDN and Edge Caching
Use when shared data, which is more static in nature needs to be delivered.

### When to Use
**[Bitly/URL Shortener](https://www.hellointerview.com/learn/system-design/problem-breakdowns/bitly)** - The read/write ratio here is extreme - one URL might be shortened once but accessed millions of times. This is a caching dream scenario. Cache the short URL to long URL mapping aggressively in Redis with no expiration (URLs don't change). Use CDN caching to handle global traffic. The actual database only gets hit for cache misses on unpopular links.

**[Ticketmaster](https://www.hellointerview.com/learn/system-design/problem-breakdowns/ticketmaster)** - Event pages get absolutely hammered when tickets go on sale. Thousands of users refresh the same event page waiting for availability. Cache event details, venue information, and seating charts aggressively. But be careful - actual seat availability can't be cached or you'll oversell. Use read replicas for browsing while keeping the write master for actual purchases.

**[News Feed Systems](https://www.hellointerview.com/learn/system-design/problem-breakdowns/fb-news-feed)** - Whether it's Facebook, LinkedIn, or Twitter, feed generation is read-intensive. Pre-compute feeds for active users, cache recent posts from followed users, and use smart pagination to avoid loading entire feeds at once. The key insight: users typically only read the first few items, so aggressive caching of recent content pays off massively.

**[YouTube/Video Platforms](https://www.hellointerview.com/learn/system-design/problem-breakdowns/youtube)** - Video metadata creates surprising read load. Every homepage visit queries for recommended videos, channel info, and view counts. Cache video metadata aggressively - titles, descriptions, and thumbnails don't change often. View counts can be eventually consistent, updated every few minutes rather than on every view. Use CDNs extensively for thumbnail images.

### When Not to Use

**Write-heavy systems** - If you're designing [Uber's location tracking](https://www.hellointerview.com/learn/system-design/problem-breakdowns/uber) where drivers update location every few seconds, focus on write scaling first. The read/write ratio might only be 2:1 or even 1:1.

**Small scale applications** - If the interviewer specifies "design for 1000 users," don't jump into complex caching strategies. A well-indexed single database handles thousands of QPS just fine. Show judgment by solving the actual problem, not the imagined one.

**Strongly consistent systems** - Financial transactions, inventory management, or any system where stale data causes real problems needs careful consideration. You might still use these patterns but with aggressive invalidation and shorter TTLs.

**Real-time collaborative systems** - [Google Docs](https://www.hellointerview.com/learn/system-design/problem-breakdowns/google-docs) and similar applications need [real-time updates](https://www.hellointerview.com/learn/system-design/patterns/realtime-updates) more than read scaling. Caching actively hurts when every keystroke needs to be immediately visible to other users.

Remember that read scaling is about reducing database load, not just making things faster. If your database handles the load fine but you need lower latency, that's a different problem with different solutions (like edge computing or service mesh optimization).

### Common deep dives
- What happens when your queries start taking longer as your dataset grows?
	- Indexing
- How do you handle millions of concurrent reads for the same cached data?
	- **Request coalescing** - combining multiple requests for the same key into a single request.
	- **Distribute hot key.** *Tradeoffs*: Duplicates, memory usage, cache consistency, but still okay as read heavy systems can benefit from it.
- What happens when multiple requests try to rebuild an expired cache entry simultaneously?
	- Use a **distributed lock** to check who is rebuilding the cache key
	- **Probabilistic cache fetch**
- How do you handle cache invalidation when data updates need to be immediately visible?
	- We can use **write through**, but the problem is the failure in the invalidation, or a new request comes before the changes are flushed in the db, cache still gets the old data here. Also, what all cache you will invalidate.
	- **Cache Versioning:** we store a data version somewhere and before looking into the cache we fetch this version and use they cache key: key+version to fetch. Version number is stored in the db -> 2 cache lookup. If does not exist, then fetch from db. Versioning works when the version is cheaper to get than the data: it’s cached, shared across many keys, or embedded in a reference the client already has. For a simple row lookup, I’d use delete-on-write with a short TTL. When versioning isn't practical—perhaps you're caching search results or aggregated data—you need explicit invalidation strategies.
	- **Deleted Item Cache:** Cache for the deleted item is maintained. Make sure that this data is small so it is easy to query.

### Related to
- [[News Feed - Twitter Timeline]]
- 