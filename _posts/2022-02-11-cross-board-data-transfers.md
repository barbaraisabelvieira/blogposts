---
toc: true
layout: post
description: International data transfers in the context of Schrems II.
categories: [digital-certificates,cryptography,encryption]
title: Almost 2 years of Schrems II! 
image: images/n-a.jpg
hide: true #hiding blog post for now 
search_exclude: true
---

With the Schrems II ruling and the new EDPB guidelines I keep reading and hearing the same thing everywhere: 
Pseudonimisation is the only technical measure that can be implemented to mitigate issues that derive from the Schrems II. 
Pseudonimisation is often seen as the holy grail and nothing else can be done. As usual, I like to challenge this statements and remind everyone that privacy by design and by default does not rely on a single measure or pattern. 

This week [CNIL deliberated](https://www.cnil.fr/fr/utilisation-de-google-analytics-et-transferts-de-donnees-vers-les-etats-unis-la-cnil-met-en-demeure) that the usage of Google Analytics was illegal. The the [Austrian Data Protection Authority (DPA)](https://www.natlawreview.com/article/austrian-dpa-finds-data-transfers-resulting-analytics-cookie-use-to-be-violation) reached to the same conclusion last month.
Today the European Data Protection Office (EDPO) shared a newspaper article from [Politico](https://www.politico.eu/article/us-eu-data-transfers-on-life-support-after-french-google-decision/) where the authors question how much longer the European DPAs will close <em> their eyes </em> to unlawful data transfers of Europeans citizens personal data to the US.
[Facebook earlier this week](https://about.fb.com/news/2022/02/meta-is-absolutely-not-threatening-to-leave-europe/) stated that if the legislation doesn't change with regards to EU cross-boarder data transfers, it becomes almost impossible for Meta to keep operating in Europe.

The Schrems II ruling did bring a lot of challenges to organisations. Even after the EDPB guidelines on cross-boarder data transfers, organisations are clearly lost and don't really known what to do next. Some organisations still keep waiting for the politicians to solve this problem and disregarded the implementation of any technical measures. 
While there's a big pressure on the politicians (like the Facebook case, for instance), most professionals working in the privacy field believe that if a Privacy Shield II will come into effect, it will be just a matter of time until it gets invalidated again.

After more than 1.5 year discussing Schrems II related issues and designing and identifying technical mitigations to be implemented by organisations, I believe it's good to make a point of the current situation. 

Please note that I don't have a legal background. Despite the trigger for this post is a legal issue, this is a purely technical post, where I try to address possible technical solutions I have identified, together with my colleagues, to address Schrems II related issues when using public cloud services. Why is this post focused on the public cloud services and Schrems II? Because that is exactly where the problem most organisations lies.

# Public cloud services and Schrems II

Schrems II is definitely pulling back organisations digital transformations. It's preventing and jeopardizing migrations to more a sustainable and maintainable IT infrastructure and services. 


Most European IT companies are currently migrating or have migrated their services and applications to US-based Cloud Service Providers (CSPs). US is one of the countries with problematic legislation with regards to data protection. So cloud native services of CSPs like Azure, AWS or GPC can become problematic when used to process personal data of European citizens. For instance, PaaS or SaaS services tend to be particularly troublesome, because privacy-friendly mitigations are often non-existent or very hard to implement. 
Why is that? When using PaaS and SaaS services, the access management these services is managed by the CSP. This is because cloud services have been designed in this way. In fact, this roots from the security design principles: it's easier to manage, easier to maintain, and much more secure. Centralisation is considered in a security world a huge asset. Think about the Log4J issue. If all the 3rd party software dependencies were centrally managed in an organisation, it was much easier to recover of the Log4J hell-week where a new version of the library would come out every 2 days.     


 For instance, 
Control over data in transit -  TLS/SSL connections are fully management and controlled by the CSP in PaaS and SaaS. If no additional layer of encryption is added, services mostly rely on the (native) secure connections provided by the CSP to establish the communication. Since the CSP manages and controls the keys and certificates used to setup the connections, the CSP also has control on how the data in transit is protected and to whom is/can be disclosed.
Control of encryption keys: The key material used to protect the data (e.g.: encryption keys, certificate keys, etc.) is stored using native keystores provided of the CSP (e.g.: Azure Key Vault, AWS KMS). These keystores are under the full control of the CSP. More specifically in SaaS, the customer has no control over cryptographic key usage and key lifecycle management. 
Control of IAM services: The CSP controls who has access to the data being processed by those services; PaaS and SaaS services natively integrate with the IAM services of the CSP (e.g.: Active Directory services). 

When the CSPs are under a problematic legislation, the data exporters (exporting data under GDPR legislation) cannot guarantee that the personal data being processed by these SaaS or PaaS services does not end up in the wrong hands (i.e., GDPR compliant). 
The question is then:
How to outsource the processing of the personal data under your control to a CSPs under problematic legislation and still guarantee the adequate level protection of the data?
The answer lies in Privacy Engineering: embedding privacy by design and by default in the software systems. Let's see where we stand with regards to Privacy Engineering.
Privacy engineering
Privacy engineering is unfortunately in its infancy. Let me explain why. For the last 20 years we've been trying to improve the security of our software systems and applications. Despite all the initiatives and methodologies that have arisen, security is still hard to implement (you can confirm this by checking this page). Over the past 5 years we see a tremendous increase of the data breaches reported. These data breaches resulted from security vulnerabilities in the systems. 
Of course, the number of users and systems and data in the digital world have dramatically increased over the last years, but you'd expect software security would also improve over the years. 
Well, lets now focus on privacy. Privacy is not Security, but without security we can't have privacy. I see privacy, from an engineering perspective, as an extension of security.
Privacy engineering tends to be even more complex than security. While in software security we can define very concrete guidelines for all software systems (e.g.: access control, source code guidelines, secure connections, etc.), when implementing privacy by design, this is not the case. Privacy is context specific. Depending on the software system and the problem at hands (use case), different mitigations apply. Sometimes, these mitigations are not always well known or even properly standardized. 
Development teams do not have privacy in the back of their minds
Delivering high quality and secure software at a faster pace is the goal of any DevOps team. This is inherent to the concept of DevSecOps. These are a set of practices which aim embedding security in the DevOps process. In a a DevSecOps methodology, everyone in the team is responsible for the security of the system. So security by design and by default is part of well defined process and supported by more or less mature tools (SASTs, DASTs, etc.).
What about Privacy? 

 
# Pseudonimization and Multi-Party Computation
Pseudonimization (tokenization) services are emerging everywhere these days. Some of them are quite mature in fact (e.g.: ePeri integrates with widely used COTS and SaaS services), but we certainly can't pseudonimize all the personal data of all the applications and expect to solve the problem. Some applications actually need the data in cleartext, so pseudonimizing the data, turns the data and the applications useless.
EDPB also points out Multi-Party Computation (MPC) based solutions as being a possible solution to solve problems derived from data transfers (link here). However, MPC is so specific to a certain context, that it certainly cannot be applied to all the use cases (or systems). 

A different perspective
Let's look at this problem from a different perspective. 

Data flows in software systems
We can summarise the main data-related processes of current applications and software systems in the following way:
Data collection: data is collected from an external source or directly provided by the end-user of the system;
Data processing: the system processes the input data by executing some sort of computation;
Data storage: the data that is collected (or the result of the computation) is stored in some storage space;
Data transfer: the collected data (or the result of the computation) can then be transferred elsewhere;

In order to perform any kind of execution, a system always has to have some input data. The input data can be either provided directly by the user using the system, or collected from other external source or system. Input data is always used to perform some computation and then it can be stored is some storage space (like a database, file, etc.) or it can be transferred elsewhere.