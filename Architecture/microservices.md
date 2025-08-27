---
title: Microservices Notes
category: Architecture
tags: [microservices, distributed-systems, service-discovery, observability]
description: Notes on microservices trade-offs, definitions, and cross-cutting concerns.
status: notes
---

# Microservices

The microservices approach is all about handling a complex system, but in order to do so, the approach introduces it's own set of complexities. When you use microservices, you need to
- have to work on automated deployment
- monitoring
- logging
- dealing with failure and fault tolerance
- service discovery

There are mechanisms to handle those complexities, but they require extra effort, time and expertise.


> So my primary guideline would be don't even consider microservices unless you have a system that's too complex to manage as a monolith.
>
> Martin Fowler


Other problems that arise with microservices are:
- Multiple deployable units
- Versioning and Compatability
- Inter-service communication and Service Discovery / Configuration
- Security and Secrets


### Definitions

- health check: A health check is a mechanism to determine the health of a service or system. It typically involves checking the availability and responsiveness of the service, as well as its ability to perform its intended functions. I did something like that in the OpEx workshop, where I created a health check endpoint that returns the status of the service.
- Business health check (BHC): A business health check is a mechanism to determine the health of a business process or system. It typically involves checking the availability and responsiveness of the business process, as well as its ability to perform its intended functions. It is similar to a health check, but it focuses on the business process rather than the technical aspects of the system.
- cross-cutting concern: A cross-cutting concern is a concern that affects multiple parts of a system, such as logging, security, and monitoring. It is a concern that is not specific to a single module or service, but rather spans across the entire system. Cross-cutting concerns are often handled by using frameworks or libraries that provide a common way to handle them across the system.
- CQRS 
References:
- [Microservices Premium, Fowler](https://martinfowler.com/bliki/MicroservicePremium.html)