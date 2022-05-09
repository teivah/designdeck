![](res/designdeck.png)

## Overview

Design Deck is an **open-source collection of 160+ system design flash cards**.

It helps you prepare and succeed in your system design interview.

The topics covered are the following:
- [Cache](cache.md): eviction, locations, strategies, when to use a cache, etc.
- [Cloud](cloud.md): main cloud components
- [Database](db.md): ACID, CAP, partitioning, consistency, isolation, denormalization, etc.
- [Design](design.md): general topics such as idempotence, bloom filter, causality, asynchronous communications, vector clocks
- [HTTP](http.md): main HTTP knowledge
- [Interview](interview.md): system design interview
- [Kafka](kafka.md): main Kafka building blocks
- [Network](network.md): TCP, CORS, ping & heartbeat, OSI, etc.
- [Reliability](reliability.md): how to guarantee reliability
- [Security](security.md): main security knowledge such as TLS, cipher, encryption
- [Time](time.md): time and distributed systems

## Cards Index

### Cache

* [Cache aside](cache.md#cache-aside)
* [Cache aside vs. inline caching](cache.md#cache-aside-vs-inline-caching)
* [Cache eviction policy](cache.md#cache-eviction-policy)
* [Cache locations](cache.md#cache-locations)
* [Main metric for cache](cache.md#main-metric-for-cache)
* [Read-through](cache.md#read-through)
* [Refresh-ahead](cache.md#refresh-ahead)
* [Write through vs. write back](cache.md#write-through-vs-write-back)
* [When to use a cache](cache.md#when-to-use-a-cache)

### Cloud

* [CDN](cloud.md#cdn)

### Database

* [ACID property](db.md#acid-property)
* [Byzantine fault-tolerant](db.md#byzantine-fault-tolerant)
* [CALM theorem](db.md#calm-theorem)
* [CAP theorem](db.md#cap-theorem)
* [Causal consistency](db.md#causal-consistency)
* [Chain replication](db.md#chain-replication)
* [Concurrency control](db.md#concurrency-control)
* [Consensus](db.md#consensus)
* [Consistency models](db.md#consistency-models)
* [Consistent prefix reads](db.md#consistent-prefix-reads)
* [CQRS](db.md#cqrs)
* [CRDT](db.md#crdt)
* [DB indexes tradeoff](db.md#db-indexes-tradeoff)
* [DB internal components](db.md#db-internal-components)
* [Denormalization](db.md#denormalization)
* [Difference view & materialized view](db.md#difference-view--materialized-view)
* [Dirty read](db.md#dirty-read)
* [Dirty write](db.md#dirty-write)
* [Document vs. relational](db.md#document-vs-relational)
* [Document-partitioned vs. term-partitioned indexes](db.md#document-partitioned-vs-term-partitioned-indexes)
* [Event sourcing](db.md#event-sourcing)
* [Eventual consistency](db.md#eventual-consistency)
* [Examples of solutions offering leader election abstractions](db.md#examples-of-solutions-offering-leader-election-abstractions)
* [Federation](db.md#federation)
* [Fencing token](db.md#fencing-token)
* [Fuzzy read](db.md#fuzzy-read)
* [Gossip protocol](db.md#gossip-protocol)
* [Graph DB main use case](db.md#graph-db-main-use-case)
* [Hot spot in partitioning](db.md#hot-spot-in-partitioning)
* [In a database, strategy to handle rebalancing](db.md#in-a-database-strategy-to-handle-rebalancing)
* [Isolation level](db.md#isolation-level)
* [Key range vs. hash partitioning](db.md#key-range-vs-hash-partitioning)
* [Knee point](db.md#knee-point)
* [Leader election](db.md#leader-election)
* [Linearizability](db.md#linearizability)
* [LSM tree](db.md#lsm-tree)
* [LSM tree vs. B-tree](db.md#lsm-tree-vs-b-tree)
* [Main downside of distributed transactions](db.md#main-downside-of-distributed-transactions)
* [Main reasons to partition data](db.md#main-reasons-to-partition-data)
* [Monotonic reads consistency](db.md#monotonic-reads-consistency)
* [Monotonic writes](db.md#monotonic-writes)
* [MVCC](db.md#mvcc)
* [N+1 select problem](db.md#n1-select-problem)
* [NoSQL](db.md#nosql)
* [Optimistic concurrency control: pros and cons](db.md#optimistic-concurrency-control-pros-and-cons)
* [PACELC theorem](db.md#pacelc-theorem)
* [Partitioning (sharding)](db.md#partitioning-sharding)
* [Phantom read](db.md#phantom-read)
* [Quorum](db.md#quorum)
* [Raft](db.md#raft)
* [Read committed consistency](db.md#read-committed-consistency)
* [Read uncommitted consistency](db.md#read-uncommitted-consistency)
* [Read-after-write consistency](db.md#read-after-write-consistency)
* [Read-heavy vs. write-heavy impacts](db.md#read-heavy-vs-write-heavy-impacts)
* [Relation between replication factor, write consistency and read consistency](db.md#relation-between-replication-factor-write-consistency-and-read-consistency)
* [Relationship between causality and linearizability](db.md#relationship-between-causality-and-linearizability)
* [Replication or partition?](db.md#replication)
* [Schema-on-read vs. schema-on-write](db.md#schema-on-read-vs-schema-on-write)
* [Serializability](db.md#serializability)
* [Serializable Snapshot Isolation (SSI)](db.md#serializable-snapshot-isolation-ssi)
* [Snapshot Isolation (SI)](db.md#snapshot-isolation-si)
* [Split-brain](db.md#split-brain)
* [SSTable](db.md#sstable)
* [Strong eventual consistency](db.md#strong-eventual-consistency)
* [Synchronous replication in DB](db.md#synchronous-replication-in-db)
* [Two-phase commit (2PC)](db.md#two-phase-commit-2pc)
* [WAL](db.md#wal)
* [When to use a column-oriented store](db.md#when-to-use-a-column-oriented-store)
* [Why DB schemaless is misleading](db.md#why-db-schemaless-is-misleading)
* [Why is in-memory faster](db.md#why-is-in-memory-faster)
* [Write and read amplification](db.md#write-and-read-amplification)
* [Write skew](db.md#write-skew)
* [Write-follows-reads](db.md#write-follows-reads)

### Design

* [Auditing](design.md#auditing)
* [Bloom filter](design.md#bloom-filter)
* [Causality](design.md#causality)
* [Consistent hashing](design.md#consistent-hashing)
* [Exactly-once delivery](design.md#exactly-once-delivery)
* [Hashing](design.md#hashing)
* [Idempotent](design.md#idempotent)
* [Load balancing](design.md#load-balancing)
* [Locality](design.md#locality)
* [Log](design.md#log)
* [Log compaction](design.md#log-compaction)
* [Microservices: pros and cons](design.md#microservices-pros-and-cons)
* [Outbox pattern](design.md#outbox-pattern)
* [Rebalancing](design.md#rebalancing)
* [REST](design.md#rest)
* [REST vs. gRPC](design.md#rest-vs-grpc)
* [Saga](design.md#saga)
* [Scalability](design.md#scalability)
* [Source of truth](design.md#source-of-truth)
* [Throughput](design.md#throughput)
* [Two generals problem](design.md#two-generals-problem)
* [UUID](design.md#uuid)
* [Validation vs. verification](design.md#validation-vs-verification)
* [Vector clock](design.md#vector-clock)
* [Why asynchronous communication](design.md#why-asynchronous-communication)

### HTTP

* [Cache-control header](http.md#cache-control-header)
* [Etag](http.md#etag)
* [HLS](http.md#hls)
* [HTTP](http.md#http)
* [HTTP methods: safeness and idempotence](http.md#http-methods-safeness-and-idempotence)
* [Keep-alive](http.md#keep-alive)
* [Main HTTP 2 features](http.md#main-http-2-features)
* [Safe method](http.md#safe-method)
* [Status codes](http.md#status-codes)
* [Status code 301 vs. 302](http.md#status-code-301-vs-302)
* [Status code 409](http.md#status-code-409)
* [What happens if you type http://www.google.com in your browser](http.md#what-happens-if-you-type-httpwwwgooglecom-in-your-browser)

### Interview

* [System design interview](interview.md#system-design-interview)

### Kafka

* [Consumer types](kafka.md#consumer-types)
* [Log compaction](kafka.md#log-compaction)
* [Offset](kafka.md#offset)
* [Partition](kafka.md#partition)
* [Partition distribution](kafka.md#partition-distribution)
* [Rebalancing](kafka.md#rebalancing)
* [Segment](kafka.md#segment)
* [Shared subscription](kafka.md#shared-subscription)

### Network

* [Backpressure](network.md#backpressure)
* [CORS](network.md#cors)
* [Difference ping & heartbeat](network.md#difference-ping--heartbeat)
* [Difference TCP & UDP](network.md#difference-tcp--udp)
* [DNS](network.md#dns)
* [Health checks: active vs. passive](network.md#health-checks-active-vs-passive)
* [Layer 4 vs. layer 7 load balancer](network.md#layer-4-vs-layer-7-load-balancer)
* [OSI model](network.md#osi-model)
* [Service mesh](network.md#service-mesh)
* [TCP congestion control](network.md#tcp-congestion-control)
* [TCP connection backlog](network.md#tcp-connection-backlog)
* [TCP handshake](network.md#tcp-handshake)
* [Websocket](network.md#websocket)

### Reliability

* [Bulkhead pattern](reliability.md#bulkhead-pattern)
* [Cascading failure](reliability.md#cascading-failure)
* [Circuit breaker](reliability.md#circuit-breaker)
* [Exponential backoff](reliability.md#exponential-backoff)
* [Fallback & timeout](reliability.md#fallback--timeout)
* [Fault tolerance](reliability.md#fault-tolerance)
* [Hard vs. soft dependencies](reliability.md#hard-vs-soft-dependencies)
* [How to set a timeout](reliability.md#how-to-set-a-timeout)
* [Jitter](reliability.md#jitter)
* [Load shedding](reliability.md#load-shedding)
* [Phi-accrual failure detector](reliability.md#phi-accrual-failure-detector)
* [Rate-limiting (throttling)](reliability.md#rate-limiting-throttling)
* [Reliability](reliability.md#reliability)
* [Retry amplification](reliability.md#retry-amplification)
* [Scalability ceiling](reliability.md#scalability-ceiling)
* [Service dependency, 5 questions to ask](reliability.md#service-dependency-5-questions-to-ask)

### Security

* [Cipher](security.md#cipher)
* [Mutual TLS](security.md#mutual-tls)
* [TLS handshake](security.md#tls-handshake)
* [Two types of encryption](security.md#two-types-of-encryption)
* [What does TLS provide?](security.md#what-does-tls-provide)

### Time

* [NTP](time.md#ntp)
* [Why can't we rely on the system clock in distributed systems?](time.md#why-cant-we-rely-on-the-system-clock-in-distributed-systems)

## References

* [Designing Data-Intensive Applications](https://dataintensive.net/)
* [Understanding Distributed Systems](https://understandingdistributed.systems/)

## Additional notes

* If you're interested in an Anki deck version, please check [#1](https://github.com/teivah/designdeck/issues/1)
* If you're preparing an algorithm & data  structure interview, you can take a look at [Algo Deck](https://github.com/teivah/algodeck/)
