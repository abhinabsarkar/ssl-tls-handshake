# SSL/TLS Handshake

## Overview
The SSL or TLS handshake enables the SSL or TLS client and server to establish the secret keys with which they communicate. Every SSL/TLS connection begins with a “handshake” – the negotiation between two parties that nails down the details of how they’ll proceed. The handshake determines what cipher suite will be used to encrypt their communications, verifies the server, and establishes that a secure connection is in place before beginning the actual transfer of data. 

## TLS Handshake
The following is a standard TLS handshake when RSA key exchange algorithm is used:
1. The SSL or TLS client sends a **client hello** message that lists cryptographic information such as the SSL version number, cipher settings, session-specific data.
2. The SSL or TLS server responds with a **server hello** message that contains the CipherSuite chosen by the server from the list provided by the client, SSL/TLS version number & session-specific data. Server also sends its certificate along with the public key.
3. Client authenticates the server certificate(e.g. Common Name / Date / Issuer). Client (depending on the cipher) creates the pre-master secret for the session & encrypts it with the server's public key.
4. Client sends the encrypted pre-master secret to the server.
5. The server uses its private key to decrypt the master secret.
6. The **client and the server now both** use the pre-master key to compute a shared secret key, called as the **shared secret**.
7. Client sends a **client finished** message encrypted using the shared secret, indicating that the client part of the handshake is complete.
8. The server decrypts and verifies the message using the shared secret key. Once verified, the server sends a **server finished** message, indicating that the server part of the handshake is complete.
9. For the duration of the SSL or TLS session, the server and client exchange messages that are symmetrically encrypted with the shared secret key.

![Alt Text](/images/ssl-tls-handshake.jpg)

## What happens when request to an SSL/TLS server is made by web browser over HTTPS?
When the client makes a request to the SSL/TLS secured server, it responds back with a certificate. In order for an SSL certificate to be trusted, that certificate must have been issued by a CA. The public keys of many CAs (known as “root certificates”) are embedded in the browsers, enabling the browser to trust any TLS or SSL certificate that cryptographically chains up to one of those trusted roots. If the certificate was not issued by a trusted CA, the web browser will then check to see if the certificate of the issuing CA was issued by a trusted CA, and so on until either a trusted CA is found (at which point a trusted, secure connection will be established) or no trusted CA can be found (at which point the browser will usually display an error). Web browser will display an “Invalid certificate” or “certificate not trusted” error.

## What happens when request to an SSL/TLS server is made by a client application hosted in SSL/TLS secured server?
The client application server will have the certificate **issued by CA installed in the trusted store**. The application code will use the CA certificate on the client server to establish handshake with the API server.

If the certificate was not issued by a trusted CA, the application code will then check to see if the certificate of the issuing CA was issued by a trusted CA, and so on until either a trusted CA is found (at which point a trusted, secure connection will be established) or no trusted CA can be found (at which point it will usually give SSL handshake error).

The list of SSL certificates, from the root certificate to the end-user certificate, represents the [SSL certificate chain](/concepts/certificate-readme.md). When the end-user certificate is installed, all the intermediate certificates must be bundled and installed along with the end-user certificate. If the SSL certificate chain is invalid or broken, certificate will not be trusted by the server & it will give SSL handshake error. 

## How SSL/TLS provide confidentiality
SSL and TLS use a combination of **symmetric and asymmetric encryption** to ensure message privacy. During the SSL or TLS handshake, the SSL or TLS client and server agree an encryption algorithm and a shared secret key to be used for one session only. All messages transmitted between the SSL or TLS client and server are encrypted using that algorithm and key, ensuring that the message remains private even if it is intercepted. SSL supports a wide range of cryptographic algorithms. Because SSL and TLS use **asymmetric encryption** when transporting the shared secret key, there is no key distribution problem.

## Asymmetric & symmetric encryption
The handshake itself uses asymmetric encryption – two separate keys are used, one public and one private. Since asymmetric encryption systems have much higher overhead, they are not usable to provide full-time, real-world security. Thus, the public key is used for encryption and the private key for decryption during the handshake only, which allows the two parties to confidentially set up and exchange a newly-created “shared key”. The session itself uses this single shared key to perform symmetric encryption, and this is what makes a secure connection feasible in actual practice (the overhead is vastly lower). 

## Basic (One-Way SSL) vs Mutually Authenticated (Two-SSL) Handshake
Majority of sessions secured by TLS only require the client verify the server. However, some cipher suites will require the client to also send a certificate and public key for mutual authentication of both parties. This two-way authentication will of course add overhead to the handshake – however, in some cases (for instance, where two banks are negotiating a secure connection for fund transfers) the cipher suite will insist upon it, and the extra security is deemed worth it.

### How One-Way SSL Works?
In one way SSL, only client validates the server to ensure that it receives data from the intended server. For implementing one-way SSL, server shares its public certificate with the clients. 
Below is the high level description of the steps involved in establishment of connection and transfer of data between a client and server in case of one-way SSL:
1. Client requests for some protected data from the server on HTTPS protocol. This initiates SSL/TLS handshake process. 
2. Server returns its public certificate to the client along with server hello message.
3. Client validates/verifies the received certificate. Client verifies the certificate through certification authority (CA) for CA signed certificates.
4. SSL/TLS client sends the random byte string that enables both the client and the server to compute the secret key to be used for encrypting subsequent message data. The random byte string itself is encrypted with the server’s public key.
5. After agreeing on this secret key, client and server communicate further for actual data transfer by encrypting/decrypting data using this key. 
Below is the pictorial description explaining how one way ssl works:
![Alt text](/images/one-way-ssl.png)
### How Two-Way (Mutual) SSL works?
Contrary to one-way SSL; in case of two-way SSL, both client and server authenticate each other to ensure that both parties involved in the communication are trusted. Both parties share their public certificates to each other and then verification/validation is performed based on that.
 
Below is the high level description of the steps involved in establishment of connection and transfer of data between a client and server in case of two-way SSL:
1.Client requests a protected resource over HTTPS protocol and the SSL/TSL handshake process begins.
2 Server returns its public certificate to the client along with server hello. 
3. Client validates/verifies the received certificate. Client verifies the certificate through certification authority (CA) for CA signed certificates.
4. If Server certificate was validated successfully, client will provide its public certificate to the server.
5. Server validates/verifies the received certificate. Server verifies the certificate through certification authority (CA) for CA signed certificates.
6. After completion of handshake process, client and server communicate and transfer data with each other encrypted with the secret keys shared between the two during handshake. 
Below image explains the same in pictorial format:
![Alt text](/images/2-way-ssl.png)