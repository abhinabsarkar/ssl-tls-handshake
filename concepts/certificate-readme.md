# Certificate chaining

## Terminologies
* **Root certificate** - A public key certificate issued by a trusted certificate authority (CA). 
* **Certificate Authority (CA)** - An entity that issues digital certificates. Common CAs are Commodo, DigiCert, GoDaddy, etc.
* **Self-signed certificate** - A certificate that is not signed by a certificate authority (CA). It is generally used for development purposes.
* **Intermediate certificate** - A subordinate certificate issued by the trusted root specifically to issue end-entity server certificates. The result is a certificate chain that begins at the trusted root CA, through the intermediate and ending with the SSL certificate issued to you

## Chain of trust
In the SSL ecosystem, anyone can generate a signing key and sign a new certificate with that signature (self-signed certificate). However, that certificate is not considered valid unless it has been directly or indirectly signed by a trusted CA. In order for this model to work, all the participants on the game must agree on a set of CA which they trust. All operating systems and most of web browsers ship with a set of trusted CAs.

When a device validates a certificate, it compares the certificate issuer with the list of trusted CAs. If a match is not found, the client will then check to see if the certificate of the issuing CA was issued by a trusted CA, and so on until the end of the certificate chain. The final certificate of the chain, the root certificate, must be issued by a trusted Certificate Authority.

![Alt Text](/images/chain-of-trust.jpg)

## SSL/TLS certificate chain
The list of SSL/TLS certificates, from the root certificate to the end-user certificate, represents the SSL/TLS certificate chain. 

For example, you have a certificate say *abs-end-user-certificate* issued by *abs-authority* for the domain *abhinab.com* which is not a root CA & thus is not embedded in the browser. Hence this certificate cannot be trusted. *abs-certificate* utilizes a certificate issued by *abs-intermediate-authority* say *abs-intermediate-certificate*. This intermediate certificate is say issued by Root CA say **DigiCert** and named as *abs-root-certificate*. In this example, the certificate chain consists of three certificates:
1. *abs-end-user-certificate* - Issued to *abhinab.com*; Issued by *abs-authority*
2. *abs-intermediate-certificate* - Issued to *abs-authority*; Issued by **DigiCert**
3. *abs-root-certificate* - Issued to **DigiCert**; Issued by **DigiCert**

![Alt Text](/images/certificate-chain-example.jpg)

When the end-user certificate is installed, all the intermediate certificates must be bundled and installed along with the end-user certificate. If the SSL certificate chain is invalid or broken, certificate will not be trusted by the server & it will give SSL handshake error.