---
toc: true
layout: post
description: Managing digital certificates in the real world.
categories: [digital-certificates,cryptography,encryption]
title: Welcome to the Certificate Hell! 
image: images/n-a.jpg
hide: true #hiding blog post for now 
search_exclude: false
---

Digital certificates are always seen as huge burden and more than often poorly managed. DevOps teams constantly struggle with properly managing digital certificates and most of the times, these are set up in insecure ways, leading to compromised certificate keys and consequently to insecure systems. 
<!-- In this post I address the main issues present in certificate management. -->

<em>
The name of this post has a funny story. Some time ago, I was in a meeting with a couple of development teams where the goal was trying to solve problems inherent to integration of APIs. Since APIs do need to be secure, the certificates topic unavoidably came up. After a while, people got stuck in a certificate management problem and suddenly someone says out laud: "Welcome to the certificate hell!". I couldn't stop smiling in light of such a statement, but obviously this reflects what most people feel about PKI management. Managing certificates can be so painful in practice that most people avoid it, completely. So I guess this post is trying to demystify some of the misconceptions inherent to this topic, as well as, creating some awareness to some of the most common obstacles and burdens that arise from managing certificates.     
</em>

# II. Small introduction to digital certificates

If you know what a certificate and for which purposes it can be used, skip this part.

## Digital certificates 

A Digital Certificate is a piece of data that typically attests that a certain (private) key is owned (and controlled by) by a specific entity. Digital certificates are used everywhere across the entire internet and they enable the trust-chain that allows the, somewhat hassle-free setup, of secure communications. Digital certificates can also be used in other context, like enabling digital signatures, but their main goal is to uniquely and undoubtedly identify an internet service or entity. Please note that through this post I'll refer to digital certificates simply as certificates.


![]({{ site.baseurl}}/images/certificate-hell/digital-certificate.jpg "Main components of a digital certificate")

A certificate contains (among other things) the following main components:
1. **Owner details** - details like DNS name, organization name, etc.
2. **Validity** - the dates in which the certificate is valid
3. **Public Key** of the owner - the public key associated to the private key of the owner of the certificate 
4. **Signature of the Certification Authority (CA)** - a digital signature of a known and trusted CA attesting that all the information present in the certificate is indeed valid and has validated by the CA 


![]({{site.baseurl}}/images/certificate-hell/google-certificate.png " "){: height="350px" with="250px"} <!-- style="float: right"} -->
For instance, if you consider the google.com certificate (on the right side):
1. **Owner details** - `google.com`
2. **Validity** - `not before 10-01-2022` and `not after 04-04-2022` (validity of 4 months)
3. **Public Key** of the owner - ` 04 09 ... 3E` 
4. **Signature of the Certification Authority (CA)** - `30 44 ... 3E`


## Using certificates to establish secure connections

Why is this information so important? As stated in the beginning, the goal of the certificate is to identify the owner of a particular private key associated to the public key in the certificate. 
So, when a new certificate is validated by a trusted external party, the Certification Authority (CA), this means that the CA attests that entity `A` is the owner of the public/private key pair `(PubK, PrivK)`. Therefore, when the key pair is used in the context of establishing a secure communication, one can trust that the server to which they are connecting to is indeed who it says it is. 


Let's consider an example. Let's say you're trying to connect to `google.com` using your browser. Your browser will do a `GET` request to `google.com` and the Google server will reply with the certificate and a set of parameters. This only happens, because by default Google only accepts secure connections.

![]({{site.baseurl}}/images/certificate-hell/eg-secure-connections.jpg "E.g.: Using certificates to enable secure connections")

The browser will get the certificate and check if the certificate is indeed valid, i.e., the browser checks:
1. If the CA digital signature of the certificate is valid (more about validity of digital signatures can be found [here](https://en.wikipedia.org/wiki/Digital_signature))
2. If the certificate is indeed issued to `google.com`. This is a fundamental check and if the URL you're trying to access doesn't match with the Common Name in the certificate (or isn't specified in the SAN fields), the certificate won't be validated by the browser. 
3. If the current date is within the range in which the certificate is valid.
4. If the certificate has not been revoked by the CA. Revoked certificates must be completely discarded.
5. If all the certificates in the **trust chain** of Google's certificate satisfy the properties above (i.e., the sub-CA certificate and the CA certificate itself are also validated in the same way) 

## What is the chain of trust?

When the browser validates the CA certificate, it needs to do all the checks above. However, since CA certificates are self-signed, in order to validate the signature present in the CA certificate, the browser ultimately needs to check if the CA is listed in the list of trusted CAs. 
This list of trusted CAs either comes loaded in the browser (e.g.: Firefox) during installation time, or corresponds to the same list of trusted CAs of Operating System (OS) where the browser is running at (e.g.: Safari). 

![]({{site.baseurl}}/images/certificate-hell/secure-connections.jpg "Using certificates to enable secure connections")

In any of the cases, all leaf certificates (i.e. end-user certificates) have a trust chain. The chain of trust of a certificate is just list of certificates, containing the leaf certificate, the sub-CA certificates (a.k.a. intermediate CA certificates), and the root CA. PKI relies on a hierarchical chain of trust, so that the browsers only have to trust in a very few CA certificates worldwide (between 50 and 70 CA certificates in total).

Certificates can also be used to validate digital signatures. In this case, the validation process of the certificate itself, is the same as the one described above.


## Public-Key Infrastructure (PKI)

In order for all of this to work properly, what is called a [Public Key Infrastructure (PKI)](https://en.wikipedia.org/wiki/Public_key_infrastructure) needs to be in place. PKI is then:
1. Everything that relates to managing the lifecycle of certificates (creation, renewal, revocation, deletion, etc.); 
2. The hardware and software used to handle and store the certificates and 
3. The governance framework that allows managing it, including policies, standards, roles, etc.

Without a proper governance framework to manage the certificates lifecycle, the WebPKI in place these days wouldn't provide the same level o security as we're used to.
Please note that the web as we know it these days fully relies on the trust placed on how the certificates are managed. Managing certificates properly is therefore of utmost importance.

##### PKI history
If you want to know the history of the webPKI, there's a great talk from Ryan Sleevi that I'd advise you to watch. You can find the link [here](https://www.youtube.com/watch?v=fazq0v-ySCo&t=1962s&ab_channel=IETF-InternetEngineeringTaskForce). Although the talk is mainly focused in the webPKI, it gives a great overview of how PKIs have evolved through time.  

## Truststores and keystores
TODO



# II. Why managing certificates can be difficult?

Ok, so now that we know what a certificate is and which purpose it serves, let's see why people consider certificate management to be a hell of job.


## No certificate (PKI) governance 
Most IT organisations using certificates, do not implement any certificate governance. This is either because of: 
1. the dimension of the company, i.e., small companies will not invest time or money in certificates. These companies will most likely rely on LetsEncrypt certificates for any external facing service. 
2. maturity of the organisation with regards to security - companies with low security maturity are not aware of certificate management inherent issues and requirements.

But, does certificate governance matter so much? Yes it does. Most of the security problems that result from certificate handling are due to poor governance practices. 
If your organisation doesn't have in place the policies, standards and procedures to manage certificates, it means it doesn't have any PKI and you shouldn't expect it to be managed in a secure way. Certificates governance is fundamental, otherwise your organisation ends up with certificates issued by a multitude of CAs, with certificates that are too permissive (see below what I call God Certificates), or even worse, with certificates with leaked (private) keys, that were never revoked. 
 
Beware that this whole thing can be very expensive to maintain. CA's are expensive, hiring PKI experts to manage internal PKIs is hard and costly, certificate management always comes holding hands with domain management, which is in itself another hard topic to manage, etc.

## Certificate pinning - Do we really need it?
Main issues derived from pinning?


## Renewing certificates with old keys?

### The PSD2 CAs 

### God certificates 

## Certificate automation and ACME
Automatic Certificate Management Environment (ACME) is a JSON-based protocol developed by the Internet Security Research Group (which developed Let's Encrypt). ACME was developed to automate issuing of certificates. Although major certification authorities support ACME, they also support other forms of certificate automation that tends to integrate directly with major (public) could providers. Also, 
the most popular web servers, proxy servers or load balancers natively support ACME as well, which enables automated certificate renewal. 

## Truststores and chain of trust
How important is the integrity of the truststores?
 
## Further reading

Podcast with Ryan Sleevi from Security, Cryptography, Whatever... [LINK](TODO).

