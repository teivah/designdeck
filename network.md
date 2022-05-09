# Network

## Backpressure

A node limits its own rate of sending in order to avoid overloading. Queueing is done on the sender side.

Also known as flow control

Example: flow control in TCP where the receiver communicates back to the sender the size of the buffer when acknowledging a segment

## CORS

Mechanism to allows restricted resources on a page to be requested from another domain, outside the domain from which the resource was served

It extends and adds flexibility to SOP (Same-Origin Policy, same domain)

Example: User visits A and the page attempts to fetch data from B:
1. Browser sends a GET request to B with Origin header A
2. Server may respond with:
- Access-Control-Allow-Origin (ACAO) header set to the domain A
- ACAO set to a wildcard (*) indicating that the requests from all domains are allowed
- An error if the server does not allow a cross-origin request

## Difference ping & heartbeat

Ping: sends messages to a process and expects a response within a specified time period (request-reply)

Heartbeat: a process is actively notifying its peers that it's still running by sending a message (notification)

## Difference TCP & UDP

| TCP | UDP            |
|-----|----------------|
| Connection-oriented    | Connectionless |
| Reliable    | Unreliable     |
| Ordered    | Unordered      |
| Heavyweight    | Leightweight               |

## DNS

Domain Name System: automatic translation between a name and an IP address

Hierarchical system

Note that caches are used to speed up the process: the browser, the OS and the DNS resolver all use caches internally

A TTL is used to inform the cache how long the entry is valid

## Health checks: active vs. passive

Passive: performed by the load balances as it routes incoming requests (e.g., HTTP status code 503)

Active: the load balancer actively checking the health of the servers via a query to their health endpoint

## Layer 4 vs. layer 7 load balancer

Layer 4 is faster and requires less computing resources than layer 7 is but less flexible

Layer 4: look at the info at the transport layer to distribute the requests (source, destination, port)

Forward packet using NAT

Layer 7: look at the info at the application layer to distribute the requests (header, message, etc.)

Terminate the network traffic, read then open a connection to the target server

A layer 7 can de-multiplex individual HTTP requests where multiple concurrent streams are multiplexed on the same TCP connection

## OSI model

7 layers: physical, data link, network (IP), transport (TCP, UDP), session, presentation, application (HTTP)

## Service mesh

All network traffic from a client goes through a process co-located on the same machine (sidecar)

Used to facilitate service-to-service communications

## TCP congestion control

Determine dynamically the throughput (the number of segments that can be sent without an ack):
- Increase exponentially for every segment ack
- Decrease with a missed ack

Upon a new connection, the size of the window is set to a system default

It's one of the reasons why reusing a TCP connection leads to a performance increase

## TCP connection backlog

SYN requests are queued before being accepted by a user-mode process

When there are too many requests for the process, the backlog reaches a limit and SYN packets are dropped (to be later retransmitted by the client)

## TCP handshake

3-way handshake
- syn (sender to receiver)
- syn-ack (receiver to sender)
- ack (sender to receiver)

## Websocket

Communication protocol (layer 7) provides a full-duplex communication channel over a single TCP connection

Different from [HTTP](http.md#http) but compatible with HTTP (starts as an HTTP connection and then is upgraded via a well-defined handshake to a TCP connection)
