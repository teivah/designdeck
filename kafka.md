# Kafka

## Consumer types

Without consumer group: each consumer will receive all the messages in a topic

With consumer group: each consumer will receive a subset of the messages

Each consumer is assigned to multiple [partitions](#partition) (zero to many)

A partition is always assigned to only one consumer

If there are more consumers than partitions, some consumers will not be assigned to any partition ([scalability ceiling](reliability.md#scalability-ceiling))

## Log compaction

Log compaction is a mechanism to give per-record retention to a topic

It ensures that Kafka will always retain at least the last message for each key of a given [partition](#partition)

A partition that is not yet compacted may have more than one message with the same key

Property:
- `retention.ms`: maximum time the topic will retain old log [segments](#segment) before deleting or compacting them (default: 7 days)

For low-throughput topic (topics with segments that should be rolled out because of `segment.ms` rather than `segment.bytes`), we should ensure that `segment.ms` is lower than `retention.ms`

## Offset

A strictly increasing identifier per [partition](#partition)

## Partition

Topics are divided into partitions

A partition is an ordered, immutable [log](design.md#log) of messages

No guaranteed ordering per topic with multiple partitions

Yet, the ordering is guaranteed per partition

## Partition distribution

The client implements a partitioner based on the key (e.g., `hash(key) % number of partitions`)

This is not done on Kafka's side

Default hash:
- In Java: murmur2
- In Go: FNV-1a

If key is empty: round-robin

## Rebalancing

Not possible to decrease the number of [partitions](#partition): topic has to be recreated

Possible to increase the number of partitions

Possible issue: no more guaranteed ordering as one key may be assigned to a different partition

## Segment

Each partition is divided into segments

Instead of storing all the messages of a [partition](#partition) in a single file, Kafka splits them into chunks called segments

A log segment is a file identified by the first message offset it contains

Properties:
- _segment.bytes_: maximum segment file size before creating a new [segment](#segment) (default: 1GB)
- _segment.ms_: period after which a new segment is created, even if the segment is not full (default: 7 days)

## Shared subscription

Distribute messages

All the consumers from one consumer group receive a portion of the messages

One [partition](#partition) is assigned to one consumer, one consumer can listen to multiple partitions
