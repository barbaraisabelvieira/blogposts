---
toc: true
layout: post
description: Bring your own key (BYOK) is a marketing feature available in most of the public cloud providers to enable the customers to use encryption keys generated by the customer.
categories: [byok,cryptography,encryption,keycontrol]
title: Please, don't Bring Your Own Key!
image: images/2021-04-bring-your-own-key/byok.jpg
hide: false
search_exclude: true
---

![]({{ site.baseurl}}/images/2021-04-bring-your-own-key/alice-bob.jpg ){: height="350px" with="300px"}


In this blog post I address Bring Your Own Key (BYOK) and the concept of (cryptographic) key control. I start by first giving a high-level overview of what BYOK feature entails and then why it is important for organisations to control the encryption keys that are used to protect the data. I then address the question of whether BYOK solves the key control problem when deploying applications in a Cloud Service Provider (CSP).


# The concept of bring your own key (BYOK)
Bring Your Own Key (BYOK) is a feature provided by the major Cloud Service Providers (CSP), where the customer generates the encryption key in its own managed Key Management System (KMS) under its full control. Then, through a set of procedures, the customer copies (or transfers) the key into the KMS managed by the CSP. From that moment on, customer applications deployed in such cloud service can use the key by directly connecting to the KMS managed by the CSP.
In this process the customer is able to fully control the key generation process. All the other key management lifecycle processes (e.g.: deployment, storage, backup, rotation, archive) are managed and controlled by the CSP.

![]({{ site.baseurl}}/images/2021-04-bring-your-own-key/byok.jpg ){: height="350px" with="300px"}


# Key control: what is it and why it is important?
Encryption is a way of preventing unauthorised access to the data. For encrypting the data, you need an encryption key.
An encryption key is just a secret value that enables you to obfuscate or deobfuscate the data into either unintelligible data and comprehensible information.

The urge to control the encryption keys comes from the fact that we need to control how the data encrypted under those keys is accessed. So, controlling how encryption keys are used means that we aim at controlling how the data is accessed.
Key control is then not just about possessing or generating the keys. Key control is about determine how keys are used (when, by whom and how).

**Having full control over the keys used to encrypt the data is then of utmost importance because in order to control how your data is being protected, you need to control how the keys are being used.**

# Does BYOK solve the problem of key control when using a Cloud Service Provider?
Let's now focus in trying to understand of whether or not BYOK solves the key control problem when you migrate your applications to the cloud.

**The straightforward answer is:** no. In fact, BYOK gives a fake sense of control.

Let's see why: the key is provided by customer, but the customer has no control on how, by whom and when, the key is used. The customer only has control over the key generation process. After copying (or transferring) the key to the KMS managed by the CSP, the customer no longer controls the key.
Note: Let's not forget that most Cloud KMS solutions are SaaS services and SaaS services are fully managed and controlled by the CSP.

Actually, if key control is the only reason you're implementing BYOK, then the advice is: do not do it. Note that BYOK incurs additional costs (and effort) in the implementation of the end solution. In alternative to BYOK, the customer can generate the encryption key directly in the KMS managed by the CSP. This is actually preferable, because it avoids exposing the keys outside of the KMS (which is a secure environment), rendering the end solution more secure.

So when the key management procedures of the CSP are followed accordingly, generating the keys on a customer managed KMS or in a CSP managed one, should provide the same level of assurance. This is actually the recommendation of some major CSPs (check here AWS explanation).

This means that there is no advantage or added value in implementing BYOK, when you are able to generate the keys in the KMS managed by the CSP.

# BYOK and Compliance Requirements
I've heard and read in many places that BYOK is a good way of solving compliance requirements with regards to key control. However, I'd really like to challenge this belief (or misbelief).

When consulting some compliance documents I wasn't able to find any document that can lead us to the conclusion that BYOK is required (or is a way) for key control. Let's check some examples.
1. The EBA guidelines on ICT and security risk management have requirements for outsourcing arrangements (section 3.2.3). But none of the controls explicitly states that BYOK is the way to achieve compliance with regards to key control. With regards to information security requirements, EBA defines that data at rest and in transit should be encrypted, as well as all the network traffic (section 3.4.4). But EBA guidelines do not make any explicit reference on how encryption keys should be managed and by whom.
2. This is also true in the case of the EDPB guidelines for data transfers. The EDPB recommendations state that the organisations must be in control of the encryption keys. But this doesn't mean, however, that BYOK comes as the solution for key control. In fact, if you take a look into use case 6, you'll notice that EDPB even considers that there are no technical solutions to protect personal data when the data is transferred to a CSP (check more detailed explanation here). This means that, in the context of data transfers under the GDPR, BYOK doesn't solve key control problem.

So if BYOK doesn't solve the key control problem, which other problem does it solve? BYOK may solve the problem of key escrow (see below). But then again, I couldn't find any compliance document that referred the need of a key escrow. Note that key escrow incurs additional risks in your implementation (see here), so you should carefully consider whether you need it or not.

# What about SaaS and BYOK?
It is also stated in many places that BYOK is a solution for solving compliance issues in SaaS solutions. As mentioned above, BYOK doesn't seem to solve any compliance requirement. Nonetheless, the same reasoning applies here, whether we're talking about a SaaS, a PaaS or a IaaS applications. As long as the KMS is managed by a 3rd party: CSP or the SaaS vendor, you have no control on how, when and by whom the key is used and accessed.
So again, if you're implementing BYOK in your SaaS solution because you believe that BYOK solves the problem of key control, then the advice is: do not do it. BYOK doesn't solve any key control problem in SaaS solutions.


# When does BYOK bring value?
If BYOK doesn't solve the key control problem, when should you use it?
1. When you need to create key escrow: for a posteriori recovery of the key, or in case the CSP is not able to guarantee key recovery, or even when your legislator demands to have a copy of the key;
2. When you need to migrate an application to the CSP and use the exactly same key (otherwise the application cannot function properly); E.g.: the migrated application needs to communicate with another application deployed in a different platform and the key is needed for the communication;

Typically, BYOK is an excellent solution when the you need to migrate your applications to a particular CSP or the application needs to be deployed across multiple CSPs.

---

# Conclusions
In a nutshell, the main highlights of this post are then the following:
1. BYOK is a feature provided by the CSP that allows importing customer generated keys into the CSP managed environment;
2. Key control implies controlling how encryption keys are used; this is important because keys are used to control how (and by whom) the data is accessed;
3. BYOK doesn't solve the problem of key control when migrating applications to a CSP, as the CSP has full control over the environment;
4. BYOK is not a way to solve the problem of key control with regards to compliance requirements, neither the problem of key control in SaaS solutions;
5. BYOK is an excellent feature when applications are deployed across multiple CSPs and require the usage of the same key.

The conclusion is then: BYOK is an excellent (and must needed) feature that all CSPs should support, but should only be implemented when it actual adds value to the implementation and not try to solve problems related with key control.

---

**Remarks**

It goes without saying, but please note that this is my personal view on the topic and is fully based on my own experiences as a cryptographic consultant.