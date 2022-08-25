---
toc: true
layout: post
description: How to implement maintainble software security?
categories: [softwaredevelopmentlifecycle, software, devops, devsecops,security]
title: Maintainable software security II
image: images/scaling-up-appsec/ssdlc.png
hide: true
search_exclude: true
---

# Introduction 

More than 4 years ago I wrote about maintainable (software) security. At the time I have described several points that I consider should be taken into account when designing software from the security point of view. Although Martin F. presents every now and then with amazing blog posts on software architecture, I rarely see someone writing about the importance of  carefully designing and architecting your software to implement security by default. 
Over the last years, DevOps and DevSecOps became increasingly important topics when considering software development. But DevSecOps is not focused on design aspects of your software that enable you to have a more maintainable approach to software security. DevSecOps is essentially focused on implementing and enabling security controls through the software development lifecycle. No doubt, this an important aspect, but if you have to maintain a system where poor security design choices have been made in the begining, then DevSecOps won't be enough. 
I truly believe in the power of a good software architecture and despite having evaluated mainy software systems across different industries, I rarelly come across an application where I consider all the design decisions to be close to perfection. Or in the other words, a software architecture that takes into account ALL the functional and non-functional requirements with brilliance and excellence. Don't get me wrong here, I had come across brilliant design aspects of many different applications, but one that combines all of them is really rare to see. This is because we probably don't write or discuss this topic enough, specially when considering security. Discussing these topics, sharing knownledge is something that benefits us all. As in everything in life is experience that allows to excel in this area. You can foresee what you're not aware off or what you have experienced. Sharing is fucral and needed. 

# Software complexity 
Software complexity introduces several long term risks:
- knowledge continuity - difficult control flows are harder to understand creating huge knowledge gaps across team members 
- lack of portability - ?!
- technical debt
- long patching and fixe times 
- security issues that are hard to identify earlier in the process 

