# DB

## 3 main reasons to partition data

- Scalability
- Improve performance of write heavy systems (usually, for example, key range partitioning can improve reads)
- Dataset doesn’t fit into a single node

## ACID property

* Atomic: all transaction succeeds or none does (all or nothing)

* Consistency: from one valid state to another (invariants must always be true)

    Not necessarily a property of the DB (e.g., foreign key constraint), can be a property of the application (e.g., credits and debits must be balanced)

    Different from consistency in eventual consistency (which is more about convergence as the matter is replicating data)

* Isolation: a transaction is not affected by another ongoing transaction (a transaction cannot read from another transaction that has not yet been completed)

    Refers to serializability

* Durability: once a transaction is committed, it will remain in the system

## Anti-entropy

Optimization to favor latency over consistency when writing to a DB (e.g., leaderless replication)

Background process to constantly looks for differences in data

Could be used as an alternative or in conjunction with read repair

## Byzantine fault-tolerant

A system is Byzantine fault-tolerant if it continues to operate correctly if in the case of a Bizantine's problem (some of the nodes malfunctioning, not obeying the protocol or malicious attackers).

## CALM theorem

Consistency As Logical Monotonicity

A program has a consistent, coordination-free (e.g., consensus-free) distributed implementation if and only if it is monotonic

Consistency in this context doesn't mean linearizability. It focuses on the consistency of the program's output while traditional consistency focus on the consistency of reads and writes.

In CALM, a consistent program is one that produces the same output no matter in which order the inputs are processed and despite any conflicts.

Said differently, does the implementation produce the outcome we expect despite any race condition that may arise.

## CAP theorem

Consistency, availability, partition tolerance (e.g., one node cut off from the rest of the cluster because of a network partition) => pick 2 out of 3

C refers to linearizability

## Caveat of serializability

It's possible that serial order is different from the order in which transactions were actually run (latest may not win)

If not, we need a stricter isolation level: strict serializability (serializability + linearizability)

## Chain replication

Replication protocol that uses a different topology than leader based replication protocols like Raft

Left-most process referred as the chain's head, right-most as the chain's tail:
- Client send writes to the head, which updates its local state and forwards to the next process in the chain
- Next process updates its local state and forwards to the next process in the chain
- Etc.
- Once the update is received by the tail, the ack flows back to the head which replies to the client that the write succeeded

![](res/chain-replication.png)

Fault tolerance is delegated to a dedicated component: control plane
- If head fails: the control plane removes it and makes the next as the head
- If intermediate node fails: the control plane removes it temporarily from the chain, and then adds it back eventually as the tail
- If tail fails: the control plane removes it and makes the predecessor as the new tail

Benefits:
- Strongly consistent protocol
- Reads are served from the tail without contacting other replicas first which allows a lower response time

Drawbacks:
- Writes are slower than quorum-based replication.
- A single slow node can slow down all writes.
- As reads are served from a single node, it can't be scaled horizontally. A mitigation is to allow intermediate nodes to serve reads but they can do it only if a read is considered as clean (the ack for this object has been returned to the predecessor). // The tail serves as the authority of the latest clean version

Notes:
- To avoid the overhead of having a single node handling the writes, we can find a way to shard data and handle multiple chains (see https://engineering.fb.com/2022/05/04/data-infrastructure/delta/)

## Chain replication vs. consensus

Similar consistency guarantees

Chain replication:
- Optimized for reads for CP systems
- Better read availability: a chain of n nodes can tolerate up to n-2 nodes failure

    Example with 5 nodes:
    - Chain replication: tolerate up to 3 nodes failure
    - Consensus with R=3 and W=3: tolerate up to 2 nodes failure

Consensus:
- Optimized for writes for CP systems

## Change data capture (CDC)

A datastore is selected as the authoritative source of data where all update operations are performed

An event log is then created from this datastore that is consumed by all the remaining operations the same way as in event sourcing

## Concurrency control

Ensures that correct results for concurrent operations are generated

Pessimistic: lock (mutual exclusion)

Optimistic: checks for conflicts at the end of a transaction

In the end, concurrency control serves the same purpose as atomicity

## Consensus

Set of processes agreeing on some data value in a fault-tolerant way

Satisfies safety and liveness

## Consistency models

Describe what expectations clients might have in terms of possible returned values despite the existence of multiple copies of data and concurrent access to it

Not the C in ACID but the C in CAP (converging to an end state)

![](res/consistency-models.png)

* Eventual consistency: all the nodes converge to the same state (not necessarily the latest)

* Write follow reads: ensures that writes are ordered after writes that were observed by previous read operations

    Example:
  - P1 reads value => foo
  - P1 updates value to bar
    => Every node will converge to bar (a process can't read bar, then foo, regardless of the process)
    Also known as session causality

* Monotonic reads consistency: a client doing several reads in sequence will never go backward in time

* Monotonic writes consistency: values originating from the same client appear in the order the client has executed them

* Read-after-write-consistency: if a client performs a write, then this write if visible during subsequent reads

    Also known as read-your-writes consistency

* Causal consistency: operations that are causally related need to be seen in the same order by all the nodes

* Sequential consistency: operations appear to take place in some total order, and that order is consistent with the order of operations from each individual clients

    Twitter example: no guarantee between which tweet is seen first between two friends posting at the same time, but the ordering is guaranteed for the same friend

* Linearizability: make a system appear as if there is only a single copy of the data and all operations are atomic (one operation at a time)

    Even though there may be multiple replicas, the application does not need to worry about them 

    C in CAP

    Real time guarantees

## CQRS

Command Query Responsibility Segregation

Dissociate writes (command) from reads (query)

Pros:
- Allows creating stores per use case (e.g., analytics, geospatial)
- Scale the read and write parts independently

Cons:
- Eventual consistency between the stores

## CRDT

Conflict-free Replicated Data Types

Data structure that is replicated across nodes:
- Replicas are updated independently, concurrently and without coordination
- An algo (part of the data type) can perform a deterministic conflict resolution
- Replicas are guaranteed to eventually converge to the same state
  => Strong eventual consistency

Used in the context of collaborative applications

Note: CRDTs can be combined to form new CRDTs

## CRDT and collaborative applications (e.g., Google Docs)

Compared to OT, each character has a stable identifier (even if characters are added or deleted)

Example: 0 is the beginning of the document, 1 is the end of the document, every character has a fractional number as an ID

May lead to interleaving problems (e.g;, two inserted words by two users are interleaved: "Alice", "Bob" => "BoAlibce"

Interleaving depends on the merging algorithm used (e.g., Treedoc doesn't lead to interleaving)

## DB indexes tradeoff

Speed up read query but slow down writes

## DB internal components

- Transport layer accepting requests
- Query processor determining the most efficient way to run queries
- Execution engine
- Storage engine

![](res/db-internal-components.png)

## DB: read vs. write-heavy, latency vs. consistency, availability vs. consistency, ACID vs. non-ACID

![](res/db.png)

## Delta CRDTs

Optimized state-based CRDTs where only recently applied changes to a state are replicated instead of the full state

## Denormalization

Introduce some amount of duplication in a normalized dataset in order to speed up reads (e.g., denormalized document, cache or index)

Cons:
- Requires more space
- May slow down writes

## Design consideration when partitioning data

Should match the primary access pattern

## Downside of distributed transactions

Performance penalty

Example: distributed transactions in MySQL are reported to be over 10 times slower than single-node transactions

## Event sourcing

Ensures that all changes to application state are stored as a sequence of events

## Eventual consistency requirements

- Eventual delivery: every update applied at a replica is eventually applied to all replicas
- Convergence: guarantees that replicas have applied the same updates eventually reach the same state

## Examples of solutions offering leader election abstractions

- etcd (linearizable)
- ZooKeeper (not linearizable for read operations)

## Federation

Splits up DB by function

## Fencing token

Monotonically increasing token that increments whenever a client acquires a distributed lock

Use case: when writing to a DB, if the provided token has a lower value than the current one, rejects the write

Solve possible issues with lease as an update has to be made from the latest token

## Gossip protocol

Peer-to-peer protocol based on the way epidemics spread

No central registry and the only way to spread common data is to rely on each member to pass it along to their neighbors

Useful when broadcasting to a large number of processes like thousands or more, where a deterministic protocol wouldn't scale

## Graph DB main use case

Relational can handle simple cases of many-to-many relationships

Yet, if the connections become more complex, it's more natural to start modeling data as a graph

## Hinted handoff

Optimization to favor latency over consistency when writing to a DB

If a coordinator node cannot contact the necessary number of replicas, it stores locally the result of the operation and forward it to the failed node(s) after they recovered

Used in sloppy quorums

## Hot spot in partitioning

Partition is heavily loaded compared to others

Also called skew

## In a database, strategy to handle rebalancing

Not based on key hashing as a rebalancing would be huge

Simple solution: Create many more partitions than nodes and assign several partitions to each node (e.g., a db running on a cluster of 10 nodes may be split into 10k partitions). When a node is added to the cluster, it will steal a few partitions from every existing node

## Isolation levels

Degree to which transactions are isolated from other concurrent execution transactions

Isolations come at a performance cost (more coordination and synchronization)

![](res/isolation-levels.png)

* Dirty writes: a transaction overwrites a value that has previously been written by another transaction that is still in-flight and has not been committed yet

    => Can violate integrity constraints

* Dirty reads: a transaction observes a write from a transaction that hasn't been committed yet

    => decisions can be taken based on data updates that can be rolled back

* Fuzzy reads: a transaction reads a value twice but sees a different value in each read because a committed transaction updated the value between the two reads

* Lost updates: two transactions reads the same value and then try to update it to two different values, only one update survives

    Example: Two transactions read the current inventory size (say 100 items), add respectively 5 and 10 items and then store back the size. Depending on the execution order, then final order can be 110 instead of 115.

* Read skew: an integrity constraint seems to be violated because a transaction can only see partial results of another transaction

* Write skew: when two transactions read the same objects, and then updates some of those objects

    Example: Two on-call doctors for a shift. Both feeling unwell, and decide to request leave. They both click the button at the same time. In the case of a write skew, the two transactions can succeed as for both, when reading the number of available doctors, it was more than one.

* Phantom reads: when a transaction does a predicate-based read and another transaction writes or removes a data matched by this predicate while the first transaction is still in flight

    Example: Transaction A computes the max and average age of employees. Transaction B is interleaved and inserts a lot of old employees. Thus, the average age could be larger than the max.

## Known CRDTs

Counter:
- Grow-only counter: increment only
- Positive-negative counter: increment and decrement (combination of two grow only counter: one positive, one negative)

Register (a memory cell storing whatever):
- LWW-register: total order using timestamps
- Multi-value register: keep track of causality, in case of conflicts it returns all conflicting cases (analogy: Git with an interactive merge resolution)

Set:
- Grow-only set: once an element is added it can't be removed
- Two-phase set: elements can be added and removed (combination of two grow only set)
- LWW-element set (last-write-wins): similar to two-phase set but we associate a timestamp for each element to resolve conflicts
- Observed-remove set: use tags instead of timestamps; each element is associated to a list of add-tags and a list of remove-tags (example: vector clocks)
- Sequence: used to build collaborative applications (e.g., Treedoc)

## Last-write-wins (LWW)

Conflict resolution based on timestamp

Used by DynamoDB or Cassandra to resolve conflicts

Shouldn't happen in single-master replication

## Leader election

Algorithm to guarantee at most one leader at any given time (safety) and that an election eventually completes (liveness)

## LSM tree

Log-Structured Merge tree

Consists of smaller mutable memory-resident (memtable) and larger immutable disk-resident (SSTable) components

Memtables data are sorted and flushed on disk when their size reaches a configurable threshold or periodically

Because of a memtable is just a special case of buffer, durability is not guaranteed (durability must be brought by replication)

Examples: Lucene, Cassandra, Bitcask, etc.

## LSM tree vs. B-tree

LSM-tree faster for writes, slower for reads because it has to check multiple data structures (bigger read amplification): memtable and SSTable

Compaction can impact ongoing requests

B-tree faster for reads, slower for writes as it must write every piece of data at least twice in the WAL & tree itself (bigger write amplification)

Each key exists in exactly one place => easier to offer strong transactional semantics

## Main difference between consistency models and isolation levels

Consistency models: applies to single-object operations

Isolation levels: applies to multi-object operations

## Merkle tree

A tree in which every leaf is labelled with the hash of a data block:
- Level n contains the data blocks
- Level n-1 the hash of one data block
- Level n-2 the hash of 2 data blocks
- Level 1 the hash of all the data blocks

![](res/merkle-tree.png)

Efficient and secure verification of the contents of a large data structure

Allows reducing data transfered between a client and a server. For example, if we want to compare a merkle tree stored on a server with one store on the client, they can both exchange their top hash. If different, we can delve in and only get the data blocks which have changed.

## Monotonic reads consistency implementation

One way to achieve it is to make sure each user always makes their reads from the same replica

## MVCC

Multiversion Concurrency Control

A possible implementation of optimistic concurrency control and snapshot isolation level

MVCC allows reads and writes to proceed with minimal coordination on the storage level since reads can continue accessing older values until the new ones are committed

## N+1 select problem

Assuming a one-to-many relationship between 2 tables A and B => A 1-* B

If we want to iterate through all the A and for each one, print the list of B, the naive implementation would be:
- `select * from A`
- And then for each A, `select * from B where A_ID = ?`

Alternatively, we could reduce the number of rount-trips to the DB from N+1 to 2 with a simple `select * from B`

Most ORM tools prevent N+1 selects

## NoSQL: main types and main architecture principles

Key-value store, document store, column-oriented store or graph DB

- Mainly partition-based
- Leverage eventual consistency

## Operation-based CRDTs

Commutative replicated data types

Replication is made in propagating the update operation

Operations characteristics:
- Must be commutative.
- Not necessarily idempotent. If idempotent, OK. If not, it's up to the delivery layer to ensure the operations are delivered without duplication.
- Delivered in causal order.

## Operational transformation (OT): concept and main drawback

A way to handle collaborative applications

Receive update operations and depending on the operations that occur concurrently, transform them

Example:
- Initial state: "helo"
- Concurrently: user 1 inserts "l" at position 3 and user 2 inserts "!" at position 4
- If transaction for user 1 completes before the one of user 2, we end up with "hell!o" instead of "hello!"
- OT will transorm the transaction from user 2 into: insert "!" at position 5

Drawback: all the communications go through a central server (e.g., impossible with systems at scale such as Google Docs)

Replaced with CRDT

## Optimistic concurrency control: pros and cons

Perform badly if high contention as it leads to a high proportion of retry, thus making performance worse

If not much contention, it tends to perform better than pessimistic

## PACELC theorem

If case of a network partition (P): we should choose between availability (A) or consistency (C)

Else, in the absence of partition (E): we should choose between latency (L) or consistency (C)

Most systems are either:
- AP/EL
- CP/EC

## Partitioning (sharding)

Split up a large dataset that is too big for a single machine into smaller parts and spread them across several machines

Define the partition type based on the primary access pattern

## Partitioning criteria

Range partitioning: keys are sorted and a partition owns all the keys from some minimum up to some maximum (example: MySQL RANGE COLUMNS partitioning)
- Pros: efficient range queries
- Cons: Risk of hot spots, requires repartitioning to potentially split a range into two subranges if a partition gets too big

Hash partitioning: hash function is applied to each key and a partition owns a range of hashes

## Partitioning methods

Horizontal partitioning: partition by rows

Vertical partitioning: partition by columns (create tables with fewer columns)
    
Rationale: if the subtables have different access patterns (e.g., a column is a blob that we rarely consume, we can create a vertical partitioning to store this blob not on the primary disk)

Also called normalization

## Quorum

Minimum number of nodes that need to vote on an operation before it can be considered successful

Usually: majority

## Raft

Leader election and replication algorithms

### Leader election

Using a state machine to elect a leader

Each process is in one of these three states: leader, candidate (part of the election process), follower

### Replication

The leader stores the sequence of operations altering the state into a local ordered log

Then, this log is replicated across followers
Each entry is considered as committed when it has been replicated on a majority of nodes

Replication enables consensus

## Read repair

Optimization to favor latency over consistency when writing to a DB (e.g., leaderless replication)

If a coordinator node receives conflicting values from the contacted replicas (which shouldn't happen in case of single-master replication for example), it resolves the conflict by:
- Resolving the conflict (e.g., LWW)
- Forwarding it to the stale replica
- Responding to the read request

## Relation between replication factor, write consistency and read consistency

Given:
- N: number of replicas
- W: number of nodes that have to ack a write for it to succeed
- R: number of nodes that have to respond to a read operation for it to succeed

If R+W > N, the system can guarantee to return the most recent written value because there's always an overlap between read and write sets (consistency)

Notes:
- In case of read-heavy systems, we want to minimize R
- If W = 1 and R = N, durability isn't guaranteed in the presence of failure
- If W < (N+1)/2, it may leads to write conflicts (e.g., W < 2 if 3 nodes)
- If R+W <= N, weak/eventual consistency

## Replication vs. partition: impacts

Replication:
- Read-heavy
- Availability > consistency

Partition:
- Write-heavy (splitting up data across different shards)

## Schema-on-read vs. schema-on-write

Schema-on-read: implicit schema but not enforced by the DB (also called schemaless but misleading)

Schema-on-write: explicit schema, the DB ensures all writes are conforming to it (e.g., relational DB)

## Serializability

I in ACID (strong isolation level)

Equivalent to serial execution (no interleaving due to concurrent transactions)

## Serializable Snapshot Isolation (SSI)

Snapshot Isolation (SI) allows write skew

SSI is a stricter isolation level than SI preventing write skew: check at runtime for conflicts between transactions

Downside: increase the number of aborted transactions

## Single-leader, multi-leader, leaderless replication

### Single-leader

All writes go through one leader

Pro: ensure consistency

Con: all writes go through a single node (bottleneck)

### Multi-leader

Rarely makes sense within a single datacenter (benefits rarely outweigh the added complexity) but used in multi-datacenter contexts

DB must resolve the conflicts in a convergent way

Use cases:
- One leader per datacenter

![](res/multi-leader.png)

- Clients with offline operation
- Collaborative editing

Different topologies:

![](res/topologies.png)

Most used: all-to-all

Pro: not limited to the write throughput of a single node

Con: possible write conflicts

### Leaderless replication

Client sends its writes to several replicas in parallel

Read requests are also sent in parallel to multiple replicas (this way, if a write hasn't been replicated yet to one replica, it won't lead to stale data)

Rely on read repair and anti-entropy mechanisms

Rely on quorum to know how long to wait for a request (not perfect: if a write fails because we didn't reach a quorum, what shall we do about the replicas where the write has already been committed)

Examples: Cassandra, DynamoDB, Riak

Pro: throughput

Con: quorums are not perfect, provide illusion of strong consistency when  in reality, it's often not true

## Sloppy quorum

In case of a quorum of w nodes to accept a write: if we can't reach w, the DB accepts the write replicate it to nodes that aren't among the ones on which the value usually lives

Relies on hinted handoff

## Snapshot Isolation (SI)

Guarantee that all reads made in a transaction will see a consistent snapshot of the database

In practice, it reads the last committed values that existed at the time it started

Allows write skew

## Snapshot Isolation common implementation

MVCC

## SSTable

Sorted String Table, immutable components of a LSM tree

Sorted immutable data structure

It consists of 2 components: index files and data files

The index (based on a hashtable or a B-tree) holds the keys and the data entries (offsets in the data file where the actual records are located)

Data files hold records in key order

## State-based CRDTs: definition and requirements

Convergent replicated data types

Replication is made in propagating the full local state to replicas

States are merged with a function which must be:
- Commutative
- Idempotent
- Associative
  => Update monotonically increase the internal state according to some partial order rules defined (e.g., max of two values, union of two sets)

=> Delivery layer doesn't have to guarantee causal ordering nor idempotency, only eventual delivery

## Strong eventual consistency: definition and requirements

Stronger guarantee than eventual consistency

Based on the fact that we can define a deterministic outcome for any conflict

Requires:
- Eventual delivery: every update applied to a replica is eventually applied to all replicas
- Strong convergence: guarantees that replicas that have executed the same updates have the same state (with eventual consistency, the guarantee is that the replicas eventually reach the same state, once consensus is reached)

Strong convergence requires convergent replicated data types (part of CRDT family)

Main difference with eventual consistency:
- Leaderless replication
- No consensus needed, instead, it relies on a deterministic outcome for any conflict

A solution to the CAP theorem

## Three-phase commit (3PC)

Failure-resilient refinement of 2PC

Unlike 2PC, satisfies liveness but not safety

## Transaction

A unit of work performed in a database system, representing a change, which can be potentially composed of multiple operations

## Two main approaches to partition a table that has secondary indexes

Partitioning secondary indexes by document:
- Each partition maintains its own secondary index
- Write: one partition
- Query on the index: requires querying multiple partitions (scatter/gather)

Optimized from writes

Example: Elasticsearch, MongoDB, Cassandra, Riak, etc.

Partitioning secondary indexes by term:
- Global index covering all the partitions (to be replicated)
- Write: multiple partitions are updated (for resiliency)
- Query on the index: served from one partition containing the index

Optimized from reads

## Two types of CRDTs

Operation-based and state-based

Operation-based require less bandwidth

State based require less assumptions about the delivery layer

## Two-phase commit (2PC)

Protocol used to implement atomic transaction commits across multiple processes

Satisfies safety but not liveness

## WAL

Write-ahead log (or redo log)

Append-only file to which every modification must be written

Used for restoration in the event of a DB crash:
- Durability
- Atomicity (allows to identify the operations on progress and complete or undo them)

## When relational vs. when document

Relational (schema-on-write):
- Better support for joins
- Many-to-one and many-to-many relationships
- ACID

Document (schema-on-read):
- Schema flexibility
- Better performance due to locality
- Closer to the data structures used by the application
- In general not ACID
- In general write-heavy

## When to use a column-oriented store

Because columns are stored contiguously: analytical workloads (computing average values, finding trends, etc.)

Flexible schema

Limited space (storing same data type together offers a better compression ratio)

## Why DB schemaless is misleading

There is an implicit schema but not enforced by the DB

More accurate term: schema-on-read

Different from relational DB with shema-on-write where the schema is explicit and the DB ensures all written data conforms to it

Similar to dynamic vs. static type checking in a programming language

## Why is in-memory faster

Not necessarily because they don't need to read from disk (even a disk-based storage engine may never need to read from disk if enough memory)

Can be faster because they avoid the overhead of encoding in a form that can be written to disk

## Write and read amplification

Ratio of the amount of data written/read to the disk versus the amount of data intended to be written

## Write heavy and replication type

Do not rely on single-master replication as it heavily impacts the scaling of write-heavy systems

Instead, rely on leaderless replication

Trade off: consistency is harder to guarantee 
