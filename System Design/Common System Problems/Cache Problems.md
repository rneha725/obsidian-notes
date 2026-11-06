## Cache Penetration
Keys which are not present anywhere can go to cache and then to DB every time.

## Cache Avalanche

## Cache Stampede
- [ ] How is it implemented? Efforts: S
Getting many requests for a resource in cache which is expired or did not exist, sometimes say an item is updated too frequently which causes a lot of events trying to update in the cache, short TTL.
As a consequence, all these requests will go to DB server, which can overwhelm the server, it will cause performance degradation and additional load on the db server.

1. Use one request to retrieve the result from db and update the cache, while this single request fetches the key, other requests can wait.
2. When a key is going to be expired, system can flag it as 'getting updated' and it can fetch the new value. Till then other requests can wait. It is better to use an exponential backoff in this case when the requests retry.
3. When cache miss happens, if we have the stale value we can return that and another process can be held responsible for updating the key.

## Cold Cache

## Evicting Cache


## Cache Invalidation
- [ ] Add examples to understand how to apply the correct cache invalidation strategy. Efforts: S
Strategies include:
- **TTL based**: Can serve stale data, if the pattern is predictable then it can be used, it is simple.
- **Write through invalidation**: When write comes, let it invalidate the cache. Adds latency to the writes but stale data won't be served.
- **Write-behind invalidation**: When write happens, queue the invalidation. Keeps write latency intact, but we can still serve stale data.
- **Tagged invalidation**: Invalidate a specific tag records in cache when related data changes. We need to maintain the tag data in this case. Needed when data dependency is a problem.
- **Versioned keys**: Include version numbers in cache keys. Increment the version on updates, naturally invalidating old cache entries. Simple and reliable but requires version tracking.

