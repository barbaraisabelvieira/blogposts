---
toc: true
layout: post
description: Managing digital certificates in the real world.
categories: [digital-certificates,cryptography,encryption]
title: Welcome to the Certificate Hell! 
image: images/n-a.jpg
hide: true #hiding blog post for now 
search_exclude: true
---

Digital certificates, despite being of utmost importance these days, are always seen by many organizations as huge burden and more than often poorly managed. DevOps teams constantly struggle with properly managing digital certificates and most of the times set them up in insecure ways leading to leakage of certificate keys and consequently to insecure systems. 
In this post I address the main issues present in certificate management. I address what some teams refer to as "certificate hell" and try to demystify some of the misconceptions associated to certificates lifecycle. 
In fact I advocate that certificates are in reality simple to understand and easy to manage, as long as you know a thing or two about how trusted Public Key Infrastructure (PKIs) work.  

Please note that through this post I'll refer to digital certificates simply as certificates.


## Digital certificates 

A Digital Certificate is a piece of data that typically attests that a certain (private) key is owned by a specific entity. Digital certificates are used everywhere across the entire internet and they enable the trust-chain that allows the, somewhat hassle-free setup, of secure communications. Digital certificates can also be used in other context, like enable digital signatures, but their main goal is to uniquely and undoubtedly identify an internet service or entity. 


### Main components of certificates

![]({{ site.baseurl}}/images/certificate-hell/digital-certificate.jpg "Main components of a digital certificate")

A certificate contains (among other things) the following main components:
1. **Owner details** - details like DNS name, organization name, etc.
2. **Validity** - the dates in which the certificate is valid
3. **Public Key** of the owner - the public key associated to the private key of the owner of the certificate 
4. **Signature of the Certification Authority (CA)** - a digital signature of a known and trusted CA attesting that all the information present in the certificate is indeed valid and has validated by the CA 


![]({{site.baseurl}}/images/certificate-hell/google-certificate.png " "){: height="450px" with="300px" style="float: right"}
For instance, if you consider the google.com certificate (on the right side):
1. **Owner details** - `google.com`
2. **Validity** - `not before 10-01-2022` and `not after 04-04-2022` (validity of 4 months)
3. **Public Key** of the owner - ` 04 09 ... 3E` 
4. **Signature of the Certification Authority (CA)** - `30 44 ... 3E`

### Using certificates to establish secure connections

Why is this information so important? As stated in the beginning, the goal of the certificate is to identify the owner of a particular private key associated to the public key in the certificate. 
So, when new certificate is validated by a trusted external party, the Certification Authority (CA), this means that the CA attests that entity `A` is the owner of the public/private key pair `(PubK, PrivK)`. Therefore, when the key pair is used in the context of establishing a secure communication, one can trust that the server to which they are connecting to is indeed who it says it is. 


Let's consider an example. Let's say you're trying to connect to `google.com` using your browser. Your browser will to a `GET` request to `google.com` and the Google server will reply with the certificate and a set of parameters. This only happens, because by default Google only accepts secure connections.

![]({{site.baseurl}}/images/certificate-hell/eg-secure-connections.jpg "E.g.: Using certificates to enable secure connections")

The browser will get the certificate and check if the certificate is indeed valid, i.e., the browser checks:
1. If the CA digital signature of the certificate is valid (more about validity of digital signatures can be found [here](https://en.wikipedia.org/wiki/Digital_signature))
2. If the certificate is indeed issued to `google.com`. This is a fundamental check and if the URL you're trying to access doesn't match with the Common Name in the certificate (or isn't specified in the SAN fields), the certificate won't be validated by the browser. 
3. If the current date is within the range in which the certificate is valid.
4. If the certificate has not been revoked by the CA. Revoked certificates must be completely discarded.
5. That all the certificates in the **trust chain** of Google's certificate satisfy the properties above (i.e., the sub-CA certificate and the CA certificate itself are also validated in the same way) 

### What is the trust-chain?


When the browser validates the CA certificate, it needs to do all the checks above. However, since CA certificates are self-signed, in order to validate the signature present in the CA certificate, the browser ultimately needs to check if the CA is listed in the list of trusted CAs. 
This list of trusted CAs either comes loaded in the browser (e.g.:Firefox) during installation time, or corresponds to the same list of trusted CAs of Operating System (OS) where the browser is running at (e.g.: Safari). 

![]({{site.baseurl}}/images/certificate-hell/secure-connections.jpg "Using certificates to enable secure connections")

In any of the cases, all leaf certificates (i.e. end-user certificates) have a trust chain. The chain of trust of a certificate chain is just list of certificates, containing leaf certificate, the sub-CA certificates (a.k.a. intermediate CA certificates), and the root CA. PKI relies on a hierarchical chain of trust, so that the browsers only have to trust in a very few CA certificates.

## The role of digital certificates when setting up secure connections



## Trusted Public-Key Infrastructure (PKI)

## Main issues


### What are truststores?

### Issues derived from pinning certificates

### Renewing certificates with old keys?

### The PSD2 CAs 

## Further reading

Podcast with Ryan Sleevi.

