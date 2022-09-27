# Cache

## Cache aside

Application is responsible for reading and writing to the DB (using write-through or write-back policy)

The cache doesn't interact with the storage directly

![](res/cache-aside.png)

## Cache aside vs. read-through

Cache aside:
- Data model can be different from DB

Read-through:
- Same data model as DB
- Can use the refresh-ahead pattern

## Cache eviction policy

- LRU (Least Recently Used)
- LFU (Least Frequently Used)
- FIFO

## Cache locations

- Client caching
- CDN
- In memory
- Distributed cache
- Database caching (query or object)

## Cache: refresh-ahead

Cache to automatically refresh any recently accessed entry prior to its expiration

Used with read-through cache

* Pro: can result in reduced latency
* Con: not accurately predicting which items are likely to be needed in the future

## Cache: write through vs. write back

Main difference: consistency

Write through:
1. Write to the cache and the DB in a single DB transaction (may still lead to cache inconsistency if the DB commit failed)
2. Return

Write back:
1. Write to the cache
2. Return
3. Asynchronously store in DB

## Four main distributed cache benefits

- Improve read latency
- Can improve availability (e.g., DB unavailable, responses are served from the cache)
- Save computation time (e.g., SQL computation)
- Independently scalable from the rest of the system

## Main metric for cache

Cache hit ratio: hits / total accesses

## Read-through cache

Read-through cache sits in-line with the DB

Single entry point

## When to use a cache

- Speed up reads
- Response complex to compute
