# Reliability

## Bulkhead pattern

Provides guaranteed fault isolation by design

Based on the idea of partitioning a shared resource to isolate failures

## Cascading failure

A process in a system of interconnected parts in which the failure of one or few parts can trigger the failure of other parts and so on

## Circuit breaker

Used to prevent a network or service failure from cascading to other failures
Implemented on the client-side

Three states:
- Closed: accept requests
- Open: do not accept requests and fail immediately
- Half-open: give the service another chance (can also be implemented using a probe)

The circuit can be opened when the health endpoint of the service is down or when the number of consecutive errors reaches a threshold

## Exponential backoff

Wait time increased exponentially after every attempt

## Fallback & timeout

When a service uses a fallback (be it a default value or a call to another service, for example), a best practice can be to ask the client to provide a timeout to define "how long before to fallback"

Indeed, some clients may want to wait longer than others before to get a fallback, so it should be configurable per client

## Fault tolerance

Property of a system that can continue operating correctly in the presence of failure of its components

## Hard vs. soft dependencies

Hard dependency: one that has to be reliable for your service to be reliable (e.g., DB)

Soft dependency: one that your service needs to operate optimally but that it can still be reliable without

Converting hard dependencies into soft ones is one of the best steps to make your service more reliable

## How to set a timeout

One idea could be based on the desired false timeout rate

For example, if we want about 0.1% false timeouts, then we should set the timeout to the 99.9th percentile of the remote call's response time

## Jitter

Introduces a part of randomness to avoid synchronized retry spikes experienced during cascading failures

## Phi-accrual failure detector

Instead of treating failure node failure as a binary problem (up or down), a phi-accrual failure detector has a continuous scale, capturing the probability of the monitored process's crash

Works by maintaining a sliding window, collecting arrival times of the most recent heartbeats

Used to approximate the arrival time of the next heartbeat and compute a suspicion level (how certain the failure detector is about a failure)

## Rate-limiting (throttling)

Mechanism that rejects a request when a specific quota is exceeded

## Reliability

Reliability

## Retry amplification

Having retries at multiple levels of the dependency chain can amplify the number of retry

The deeper a service in the chain, the higher the load it will be exposed to due to amplification:

![](res/retry-amplification.png)

Source: [Understanding Distributed Systems](https://understandingdistributed.systems/)

In case of a long dependency chain, perhaps we should only retry at a single level of the chain

## Scalability

System's ability to cope with increased load

## Scalability ceiling

Hard limit (e.g., device maximum throughput)

## Service dependency, 5 questions to ask

- Timeout?
- Retry policy?
- Circuit breaker?
- Reasonable fallback value in case of failure?
- Can we defer the work and try again later?
