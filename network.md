# Network

## ARP protocol

Map an IP address to a MAC address

## Average connection speed in USA

42 Mbps

## Backpressure

A node limits its own rate of sending in order to avoid overloading. Queueing is done on the sender side.

Also known as flow control

Example: TCP flow control

## Bandwidth

Maximum amount of data that can be transferred in a unit of time

## BGP

Border Gateway Protocol: Routing system of the internet

When a client submits data via the Internet, BGP is responsible for looking at all of the available paths that data could travel and picking the best route

Note: The chosen route isn't necessarily the fastest one, it can be the cheapest one. See https://technology.riotgames.com/news/fixing-internet-real-time-applications-part-i.

## CORS

Cross-origin resource sharing

Mechanism to allow restricted resources on a page to be requested from another domain outside the domain from which the resource was served

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

* Connection-oriented / Connectionless
* Reliable / Unreliable
* Ordered / Unordered
* Heavyweight / Leightweight

## Difference view & materialized view

A view is just an abstraction (SQL request is rewritten to match the actual schema)

A materialized view is a copy (written to disk)

## DNS

Domain Name System: automatic translation between a name and an IP address

![](res/dns.png)

Notes:
- Usually the local DNS configuration is the ISP one (config initialized from the router or static config)
- The browser, the OS and the DNS resolver all use caches internally
- A TTL is used to inform the cache how long the entry is valid

## DNS lookup: push or pull

DNS is based on the pull mode:
- If record is present: DNS will return it
- If record isn't present: DNS will pull the value, store it, and then return it

Notes:
- New DNS records are immediate
- DNS updates are slow because of TTL (there is no propagation, we wait for cached records to expire)

## Health checks: passive vs. active

Passive: performed by the load balancer as it routes incoming requests (e.g., 503)

Active: the load balancer actively checking the health of the servers via a query to their health endpoint

## Internet model

A network of networks

![](res/internet-model.png)

## Layer 4 vs. layer 7 load balancer

Layer 4 is faster and requires less computing resources than layer 7 is but less flexible

Layer 4: look at the info at the transport layer to distribute the requests (source, destination, port)

Forward packet using NAT

Layer 7: look at the info at the application layer to distribute the requests (header, message, etc.)

Terminate the network traffic, read then open a connection to the target server

A layer 7 can de-multiplex individual HTTP requests where multiple concurrent streams are multiplexed on the same TCP connection

## MAC address

A unique identifier assigned to a network interface

## Max size of a TCP packet

64K

## MQTT LWT

Last Will and Testament

Whenever a client is marked as disconnected (proper disconnection or heartbeat failure), it triggers to send a message in a particular topic

## NTP

Network Time Protocol: used to synchronize clocks

## OSI model

7 layers:
1. Physical: transmission of raw bits over a physical link (e.g., USB, Bluetooth)
2. Data link: responsible from moving a packet of data from one node to a neighbouring node
3. Network: provides a way of sending packets between nodes that are not directly linked and might belong to other networks (e.g., IP, iptables routing)
4. Transport: application to application communication, based on ports when multiple applications on the same node wants to communicate (e.g., TCP, UDP)
5. Session
6. Presentation
7. Application: protocol of exchanges between the two sides (e.g., DNS, HTTP)

## Routers

A way to connect networks that are connected with each other (used for the Internet)

Capable of routing packets properly across networks so that they reach their destination successfully

Based on the fact that an IP has a network prefix

## Routers buffering

Routers use queuing (buffering) to address network congestion

A buffer has a fixed size and a fixed number of packets

If no available buffer: packet is dropped

Note: not a way to increase the throughput

## Routers processing

Per-packet processing, no buffering

Impacts:
- It’s faster to route 10 packets of 1000 bytes than 20 packets of 500 bytes
- Sending small packets more frequently can fill the router buffer more quickly

Source: https://technology.riotgames.com/news/fixing-internet-real-time-applications-part-i

## Routing table

- Network destination and mask (together form the network identifier)
- Gateway: next node to which a packet has to be sent
- Interface: corresponding interface through which the gateway can be reached

Example:

| Destination | Network mask  | Gateway   | Interface |
|-------------|---------------|-----------|-----------|
| 0.0.0.0     | 0.0.0.0       | 240.1.1.3 | if1       |
| 240.1.1.0   | 255.255.255.0 | 0.0.0.0   | if1       |

## Service mesh

All network traffic from a client goes through a process co-located on the same machine (sidecar)

Used to facilitate service-to-service communications

## Switch

Receive frame and forward to specific links they are addressed to. Used for local networks.

Example: Ethernet frame

![](res/ethernet-frame.png)

To do this, the switch maintains a switch table that maps MAC addresses to the corresponding interfaces that lead to them

At first, the switch table is empty. If the entry is empty, a frame is forwarded to all the interfaces (switches are self-learning)

## TCP congestion control

Determine dynamically the throughput (the number of segments that can be sent without an ack):
- Increase exponentially for every segment ack
- Decrease with a missed ack

Upon a new connection, the size of the window is set to a system default

It's one of the reasons why reusing a TCP connection leads to a performance increase

## TCP connection backlog

SYN requests are queued before being accepted by a user-mode process

When there are too many requests for the process, the backlog reaches a limit and SYN packets are dropped (to be later retransmitted by the client)

## TCP connection termination

4-way handshake
- FIN (sender to receiver)
- ACK (receiver to sender)
- FIN (receiver to sender)
- ACK (sender to receiver)

## TCP flow control

A receiver communicates back to the sender the size of the buffer when acknowledging a segment

Backpressure mechanism

## TCP handshake

3-way handshake
- syn (sender to receiver)
- syn-ack (receiver to sender) // ack the segment number received
- ack (sender to receiver) // ack the segment number received

## Websocket

Communication protocol (layer 7) provides a full-duplex communication channel over a single TCP connection and bidirectional streaming capabilities

Different from HTTP but compatible with HTTP (starts as an HTTP connection and then is upgraded via a well-defined handshake to a TCP connection)

Obsolete with HTTP/2

## Why can't we rely on the system clock in distributed systems?

- There's no guarantee that times are synchronized
- In the case of an NTP synchronization, the system clock of one node can jump backward in time
