# HTTP

## Cache-control header

Allows setting how long to cache a response

If marked as private, the results are intended for a single user (then won't be cached by a load balancer for example)

## Etag

Entity tag header that allows clients to make conditional requests

Server returns an ETag being the date and time of the last update of a resource
Client sends a `If-Match` header to update a resource only if clients have the most recent version

## HLS

HTTP live streaming: video streaming protocol

## HTTP

Application layer protocol (OSI level 7)

Request/response protocol used to encode and transport information between a client and a server

Stateless (each request is executed independently)

The request and the response are 2 standard message types exchanged in a single HTTP transaction
- Request: method, UTL, HTTP version, headers, body
- Response: HTTP version, status, reason, headers, body

Relies on a transport protocol (OSI level 4, TCP most of the times but not mandatory) for error detection, flow control, reliability, etc.

## HTTP methods: safeness and idempotence

| Method | Safe | Idempotent |
|--------|-----|------------|
| GET    | ✅    | ✅           |
| PUT    | ❌    | ✅           |
| POST   |  ❌   | ❌           |
| DELETE |  ❌   | ✅           |

## Keep-alive

Maintain a persistent TCP connection (reduces the number of TCP handshakes)

## Main HTTP 2 features

- Request multiplexing: multiple requests over a single TCP connection
  => Prioritization can now be part of the request
- Server push
- Binary protocol (lower overhead in decoding data, smaller network footprint)
- Header compression

## Safe method

Doesn't have any visible side effects and can be cached

## Status codes

- 2xx: success
- 3xx: redirection
- 4xx: client error
- 5xx: server error

## Status code 301 vs 302

301: redirect permanently

302: redirect temporarily

## Status code 409

When clients are throttled, the most common way is to return a 429 (_Too Many Requests_)

The response can also include a _Retry-After_ header indicating how long to wait before making a new request (in seconds)

## Too many requests status code

429

## What happens if you type http://www.google.com in your browser

- DNS lookup
- TCP handshake
- HTTPS handshake
- HTTP request (GET, port 80)
