# HTTP

## 301 vs. 302

301: redirect permanently

302: redirect temporarily

## 403 or 404?

Retuning 403 can leak existence of a resource

Example: Apple is secretly working on super cars and creates an internal GET `https://apple.com/supercar` endpoint

Returning 403 means the user doesn't have the rights to access the resource, but leaks the existence of `/supercar`

## Cookie

Small files stored on a user's computer to hold specific data (e.g., language preference)

Requests made by the browser will contain cookies data

Types of cookies:
- Session cookies: only lasts for the duration of a session
- Persistent cookies: outlast user session
- Third-party cookies: used for advertising

## Four main HTTP/2 features

- Request multiplexing: multiple requests over a single TCP connection
  => Prioritization can now be part of the request
- Server push
- Binary protocol (lower overhead in decoding data, smaller network footprint)
- Header compression

## HLS

HTTP live streaming: video streaming protocol

## HTTP

Request/response protocol used to encode and transport information between a client and a server
Stateless (each request is executed independently)

The request and the response are 2 standard message types exchanged in a single HTTP transaction
- Request: method, URL, HTTP version, headers, body
- Response: HTTP version, status, reason, headers, body

Example of a POST request:

```http request
POST https://example.com HTTP/1.0
Host: example.com
User-Agent: Mozilla/4.0
Content-Length: 5

Hello
```

Application layer protocol (OSI level 7)

Relies on a transport protocol (OSI level 4, TCP most of the time but not mandatory) for error detection, flow control, reliability, etc.

## HTTP cache-control header

Allows setting how long to cache a response

Part of the response header (hence, cached by the browser) but can be part of the request header too (hence, cached on server side)

If request header marked as private, the results are intended for a single user (then won't be cached by a load balancer for example)

## HTTP Etag

Entity tag header that allows clients to make conditional requests

Server returns an ETag being the date and time of the last update of a resource

Client sends a `If-Match` header to update a resource only if clients have the most recent version

## HTTP keep-alive

Maintain a persistent TCP connection (reduces the number of TCP and HTTPS handshakes)

## HTTP methods: safeness and idempotence

- GET: safe, idempotent
- PUT: not safe, idempotent
- POST: not safe, not idempotent
- DELETE: not safe, idempotent

## HTTP safe method

Doesn't have any visible side effects and can be cached

## HTTP status code 429

When clients are throttled, the most common way is to return a 429 (Too Many Requests)

The response can also include a Retry-After header indicating how long to wait before making a new request (in seconds)

## HTTP status codes

- 2xx: success
- 3xx: redirection
- 4xx: client error
- 5xx: server error

## What happens if you type google.com in your browser

- URL parsing
- HSTS lookup (HTTP Strict Transport Security: list of websites that have requested to be contacted via HTTPS only)
- DNS lookup:
    - Is DNS record cached in browser?
    - If not present, check if the hostname can be resolved by reference in the local hosts file
    - If not present, DNS lookup (typically to the ISP DNS) // Uses ARP to get the MAC address of the DNS IP address
- Opens a TCP socket
- TCP handshake
- HTTPS handshake
- HTTP request (GET, port 80)
- Receive HTML, javascript (to be executed on client side), and images. Data can be cached by the browser using HTTP Etag.

Source: https://github.com/alex/what-happens-when
