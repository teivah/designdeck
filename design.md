# Design

## Auditing

Checking the integrity of data

## Bloom filter

Probabilistic, memory-efficient data structure for approximating the content of a set

Can tell if a key does not appear in the DB

## Causality

Causal dependency: one event causing another

## Consistent hashing

Special kind of hashing such that when a resize occurs, only 1/n percent of the keys need to be rebalanced (n: number of nodes)

## Exactly-once delivery

Impossible to achieve

However, we can achieve exactly-once processing using a dedup or by requiring the messages to be idempotent

## Hashing

Map data of arbitrary size to fixed-size values

Examples:
- MD5: 16 bytes
- SHA256: 32 bytes

## Idempotent

If executed more than once it has the same effect as if it was executed once

## Load balancing

Route requests across a pool of servers

Note: Using a balancing based on the least loaded endpoint isn't efficient. A more efficient option is to randomly pick two servers and route the request to the least-loaded one of the two.

## Load shedding

Action to reduce the load on something

Example: when the CPU utilization reaches a threshold, the server can start returning errors

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

## Outbox pattern

Used to update a DB and publish an event in a transactional fashion

Within a transaction, persist in the DB (insert, update or delete) and insert at the same time a new row in an event table

Implements a worker that checks the event table, publishes an event and deletes the row (at least once guarantee)

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

## Saga

Distributed transactions composed of a set of local transactions

Each transaction has a corresponding compensation action to undo its changes

Usually, a Saga is implemented with an orchestrator that manages the execution of the transactions and handles the compensations if needed

## Source of truth

Holds the authoritative version of the data

## Throughput

The rate of work performed

## Two generals problem

It is impossible to achieve an agreement between two parties in the presence of link failures if the communication is asynchronous

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

Reduce temporal coupling (not connected at the same time) => processes execute at independent rates

If the interaction pattern isn't request/response with client blocking until it receives the response
