# Security

## Cipher

Encryption algorithm

## Mutual TLS

Add client authentication using a certificate

## TLS handshake

With mutual TLS:

1. Client hello: protocol, cipher, etc.
2. Server hello: supported cipher, etc.
3. Server sends its certificate
4. Client checks the server certificate (e.g., make sure the CA are trusted in its truststore, etc.)
5. Client sends its certificate
6. Server checks the client certificate
7. The client generates a session key encrypted with the public key of the client certificate (asymmetric encryption)
8. Client sends data and encrypts each packet using the session key (symmetric encryption)

One way: the session key is generated by the client

## Two types of encryption

Symmetric: key is shared between a client and a server (fastest)

Asymmetric: two keys are used, a private and a public one
- Client encrypts a message with the public key
- Server decrypts the message with its private key

## What does TLS provide?

- Encryption: data is obfuscated
- Authentication: verify identity
- Integrity: protection against data tampering
