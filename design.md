# Design

## Auditing

Checking the integrity of data

## Backward vs. forward compatibility

![](res/backward-forward-compatibility.png)

## Bloom filter

Probabilistic, memory-efficient data structure for approximating the content of a set

Can tell if a key does not appear in the DB

## Causality

Causal dependency: one event causing another

Happened-before relationship

## Concurrent operations

Not only operations that happen at the same time but also operations made without knowing about each other

Example:
- Concurrent to-do list operations with a current "Buy milk" item
- User 1 deletes it
- User 2 doesn't have an internet connection, modifies it into "Buy soy milk", and then is connected again => this modification may have been done one hour after user 1 deletion

## Consistent hashing

Special kind of hashing such that when a resize occurs, only 1/n percent of the keys need to be rebalanced (n: number of nodes)

Solutions:
- Ring consistent hash with virtual nodes to improve the distribution
- Jump consistent hash: faster but nodes must be numbered sequentially (e.g., if we have 3 servers foo, bar, and baz => we can't decide to remove bar)

## Design impacts of sharing

May decrease:
- Availability
- Performance
- Scalability

## Design: read-heavy vs. write-heavy impacts

Read heavy:
- Leverage replication
- Leverage denormalization

Write heavy:
- Leverage partition (usually)
- Leverage normalization

## Different types of message failure

- Delayed
- Dropped
- Duplicated
- Out-of-order

## Event log vs. message queue

Event log:
- Consumers are free to select the point of the log they want to consume messages from, which is not necessarily the head
- Log is immutable, messages cannot be removed by consumers (removed by a GC running periodically)

## Exactly-once delivery

Impossible to achieve

However, we can achieve exactly-once processing using a dedup or by requiring the consumers to be idempotent

## FLP impossibility

In an asynchronous distributed system, there's no consensus algorithm that can satisfy:
- Agreement
- Validity
- Termination
- And fault tolerance

## Geohashing

Encode geographic coordinates into a short string called a cell with varying resolutions

The more letters in the string, the more precise the location

![](res/geohashing.png)

Main use case:
- Proximity searches in O(1)

## Hashing definition and size of MD5 and SHA256

Map data of arbitrary size to fixed-size values

Examples:
- MD5: 16 bytes
- SHA256: 32 bytes

## HDFS

Distributed filesystem:
- Fault tolerant
- Scalable
- Optimised for batch operations

Architecture:
- Single master (maintain filesystem metadata, inform clients about which server store a specific part of a file)
- Multiple data nodes

Leverage:
- Partitioning: each file is partitioned into multiple chunks => performance
- Replication => availability

Read: communicates with the master node to identify the servers containing the relevant chunks

Write: chain replication

## How to reduce sharing

- Decompose stateful and stateless parts of a system => makes scaling easier
- Partitioning => fault isolation

## HyperLogLog

Used to approximate cardinality of a set

Optimization for space over perfect accuracy

### Backing idea

Coin flip game: you flip a coin, if head, flip again, if tail stop

If a player reaches n flips, it means that on average, he tried 2n+1 times

### Algo

For an ID, we will count how many consecutive 0 (head) bits on the left

Example: 001110 => 2

Hence, on average we should have seen 22+1 visitors

Requirement: we need visitors ID to be uniform => either if the ID is randomly generated or by hashing them (if ID is auto incremented for example)

Required memory: log(log(m)) with m the number of  unique visitors

Problem with this algo: it depends on luck. For example, if user 00000001 connects every day => the system will always approximate 28 visitors

### Bucketing

Distribute to multiple counters and aggregate the results (possible because each counter is very small)

If we want 4 counters, we distribute the ID based on the first 2 bits

Result: 2(n1 + n2 + n3 + n4) / 4

Problem: mean is highly impacted with large outliers

Solution: use harmonic mean

## Idempotent

If executed more than once it has the same effect as if it was executed once

## Latency numbers every programmer should know

- Read 1MB sequentially from memory: 250 µs
- Round trip within the same datacenter: 0.5 ms
- Read 1MB sequentially from SSD: 1 ms
- Disk seek: 10 ms
- Read 1MB sequentially from disk: 20 ms
- Send round trip packet over continents: ~100 ms

## Lease

Lock with an expiry timeout after which the lock is automatically released

May lead to situations where two nodes believe they hold the lock (for example, when the expiry signal hasn't been caught yet by the first node because of a GC or CPU throttling)

Can be solved using a fencing token

## Least loaded endpoint load balancing strategy

Not efficient

A more efficient option is to randomly pick two servers and route the request to the least-loaded one of the two

## Liveness property

Something good will eventually occur

Example: leader is elected, eventual consistency

## Load balancing

Route requests across a pool of servers

## Load shedding

Action to reduce the load on something

Example: when the CPU utilization reaches a threshold, the server can start returning errors

A more special form of load shedding is selective client throttling, where an application assigns different quotas to each of its clients

## Locality

Performance optimization to put several pieces of data in the same place

## Log

Append-only, totally ordered sequence of messages

Each message is:
- Appended at the end of the log
- Is assigned a unique sequential index

Example: Kafka

## Log compaction

Throw away duplicate keys in the log and keep only the most recent update for each key

## Main drawback of shared-nothing architectures

Reduce flexibility

If the application needs to access to new data access patterns in an efficient way, it might be hard to provide it given the system's data have been partitioned in a specific way

Example: attempting to query by a secondary attribute that is not the partitioning key might require to access all the nodes of the system

## MapReduce

Programming model for processing large amounts of data in bulk across many machines:
- Map: processes a set of key/value pairs and produces as output another set of intermediate key/value pairs.
- Reduce: receives all the values for each key and returns a single value, essentially merging all the values according to some logic

## Microservices: pros and cons

Pros:
- Organizational (each team dictates its own release schedule, etc.)
- Codebase is easier to digest
- Strong boundaries
- Independent scaling
- Independent data model

Cons:
- Eventual consistency
- Remote calls
- Harder to operate (more complex)

## Number of values to generate to reach 50% chances of collision: 32-bit, 64-bit, and 128-bit hash

- 32: 80 k
- 64: 5 billion
- 128: 3e+18 (1 billion hashes generated every second for 100 years)

## Orchestration vs. choreography

Orchestration: single central system responsible for coordinating the execution

Choreography: no need for a central coordinator, each system is aware of the previous and the next

## Outbox pattern

Used to update a DB and publish an event in a transactional fashion

Within a transaction, persist in the DB (insert, update or delete) and insert at the same time a new row in an event table

Implements a worker that checks the event table, publishes an event and deletes the row (at least once guarantee)

## Perfect hashing

No collision, only possible if we know the keys up front

Given k elements, the hashing function returns an int between 0 and k

## Quadtree

Tree data structure where each internal node has exactly four children: NE, NW, SE, SW

Main use case:
- Improve geospatial caching (e.g., 1km in an urban area isn't the same as 1km outside cities)

Source: https://engblog.yext.com/post/geolocation-caching

## Rate-limiting (throttling): definition and algos

Mechanism that rejects a request when a specific quota is exceeded

### Token bucket algo

Token of a pre-defined capacity, put back in the bucket periodically:

![](res/token-bucket-algo.png)

### Leaking bucket algo

Uses a FIFO queue
When a request arrives, checks if the queue is full:
- If yes: request is dropped
- If not: added to the queue
  => Requests pulled from the queue at regular intervals

![](res/leaking-bucket-algo.png)

## Rebalancing

Move data or services from one node to another in order to spread the load fairly

## REST

Architectural style where the server exposes a set of resources

All communications must be stateless and cacheable

Relies mainly on HTTP but not mandatory

## REST vs. gRPC

REST (architectural style):
- Universality
- Standardization (status code, ETag, If-Match, etc.)

gRPC (RPC framework):
- Contract
- Binary protocol (faster, less bandwidth) // We could use HTTP/2 without gRPC and leverage binary protocols but it would require more efforts
- Bidirectional

## Safety property

Something bad will never happen

Example: leader election eventually completes

## Saga

Distributed transaction composed of a set of local transactions

Each transactions has a corresponding compensation action to undo its changes

Usually, a Saga is implemented with an orchestrator that manages the execution of the transactions and handles the compensations if needed

## Scalability

System's ability to cope with increased load

## Scalability ceiling

Hard limit (e.g., device maximum throughput)

## Shared-nothing architectures

Reduce coordination and contention so that every request can be processed independently by a single node or group of nodes

![](res/shared-nothing-architecture.png)

Increase availability, performance, and scalability

## Source of truth

Holds the authoritative version of the data

## Split-brain

Network partition => nodes unable to communicate with each other => multiple nodes believing they are the leader

As a node is unaware that another node is still functioning, it can lead to data corruption or data loss

## Throughput

The rate of work performed

## Total vs. partial order

Total order: a binary relation that can be used to compare any 2 elements of a set with each other

Partial order: a binary relation that can be used to compare only some of the elements of a set with each other

Total ordering in distributed systems is rarely mandatory

## UUID

128-bit number

Collision probability: after generating 1 billion UUID every second for ~100 years, the probability of creating a single duplicate reaches 50%

## Validation vs. verification

Validation: process of analyzing the parts of the system and building mental models that reflects the interaction of those parts

Example: validate the quality of water by inspecting all the pipes and infrastructure to capture, clean and deliver water

Verification: process of analyzing output at a system boundary

Example: validate the quality of water by testing the water (output) coming from a sink

## Vector clock

Algorithm that generates partial ordering of events and detects causality violation

## Why asynchronous communication

Reduce temporal coupling (not connected at the same time) => processes execute at independent rates, without blocking the sender

If the interaction pattern isn't request/response with client blocking until it receives the response
