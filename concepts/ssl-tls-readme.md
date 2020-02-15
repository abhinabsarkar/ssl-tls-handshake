# SSL & TLS Overview

SSL(Secure Sockets Layer) and TLS(Transport Later Security) are cryptographic protocols that authenticate data transfer between servers, systems, applications and users. SSL/TLS certificate acts as an endpoint encryption system that encrypt data preventing unauthorized access by hackers. For example, a cryptographic protocol encrypts the data that is exchanged between a web server and a user.

SSL was a first of its kind of cryptographic protocol. TLS on the other hand, was a recent upgraded version of SSL. SSL is obsolete and TLS is the current encryption standard used now.

## SSL History
Netscape developed SSL in the year 1994. It was envisioned as a system that will ensure secure communication between client and server systems on the web. Gradually, the IETF (the Internet Engineering Task Force) picked up the protocol and standardized it as a protocol.

| SSL version | Description |
| ----------- | ----------- |
| SSL 1.0 | Due to security flaw, SSL 1.0 was not released. |
| SSL 2.0 | SSL v2.0 was the first public release of SSL by Netscape. It was released in February 1995 but there were design flaws that compelled Netscape to release SSL v.3. However, SSL v.2.0 was deprecated in 2011. |
| SSL 3.0 | SSL v3 was an upgrade version of earlier version SSL v2.0 that fixed few security design flaws of SSL v2.0 However, SSL v3.0 deemed insecure in 2004 due to the POODLE attack. |

## TLS History
TLS is a successor of SSL 3.0, which was released in 1999.

| TLS version | Description |
| ----------- | ----------- |
| TSL 1.0 | TLS 1.0 which was upgrade of SSL v.3.0 released in January 1999 but it allows connection downgrade to SSL v.3.0. |
| TSL 1.1 | After that, TLS v1.1 was released in April 2006, which was an update of TLS 1.0 version. It added protection against CBC (Cipher Block Chaining) attacks. In March 2020, Google, Apple, Mozilla and Microsoft has announced for deprecation of TLS 1.0 and 1.1 versions.
| TSL 1.2 | TLS v1.2 was released in 2008 that allows to specification of hash and algorithm used by the client and server. It allows authenticated encryption, which was added more support with extra data modes. TLS 1.2 was able to verify length of data based on cipher suite. |
| TSL 1.3 | TLS v1.3 was released in August 2018 and had major features that differentiate it with its earlier version TLS v1.2 like removal of MD5 and SHA-224 support, require digital signature when earlier configuration used, compulsory use of Perfect forward secrecy in case of public-key based key exchange, handshake messages will now be encrypted after “Server Hello”. |