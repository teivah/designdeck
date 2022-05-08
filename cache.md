# Cache

## Cache aside

Application is responsible for reading and writing to the DB (using write through or write back policy)

The cache doesn't interact with the storage directly

## Cache aside vs. inline caching

Cache aside:
- No stale data as the application invalidates cache data for every write (if cache is distributed)
- May lead to consistency issue is write to the cache fail

Inline caching (cache on top of the store):
- Easier for the application (single entry point, not up to the application to handle cache misses)
- Can use the refresh-ahead pattern
- Might get stale data in case of write back

## Cache eviction policy 

- LRU (Least Recently Used)
- LFU (Least Frequently Used)
- FIFO

## Cache locations

- Client caching
- CDN
- Application caching
- Database caching (query or object)

## Main metric for cache

Cache hit ratio: hits / total accesses

## Read-through

In cache-aside, the application is responsible for populating the cache

In read-through, the logic is supported by a library or a cache provider on top of the DB

Single entry point

## Refresh-ahead

Cache to automatically refresh any recently accessed entry prior to its expiration

Used with read-through cache

Pro: can result in reduced latency

Con: not accurately predicting which items are likely to be needed in the future

## Write through vs. write back

When cache is inlined

Write through:
1. Write to the cache
2. Store in DB
3. Return

Write back:
1. Write to the cache
2. Return
3. Asynchronously store in DB

## When to use a cache

Data read frequently but modified infrequently
