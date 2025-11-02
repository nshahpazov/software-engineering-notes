

---
title: Architecture Terminology Dictionary
category: Architecture
tags: [architecture, terminology, slo, sla, sli]
description: Short glossary and reminders for architecture terminology.
status: notes
---

This document consists of 


### Service Level 

- SLO - Service Level Objective - internal team objective that the team aims towards
- SLA - Service Level Agreement - What the team has agreed on with other parties and stakeholders
- SLI - Service Level Indicator - What is the actual metric that is being observed. 

### Architecture fundamentals

- Architectural Drivers - The "why" behind a design: prioritized goals, constraints, and quality attributes that shape the architecture.
- ASR - Architecturally Significant Requirement - A requirement (functional or non-functional) that strongly influences the architecture.
- Quality Attributes (QAs) - Non-functional properties like performance, availability, security, scalability, and maintainability.
- Constraints - Hard boundaries you must respect (tech, compliance, budget, deadlines).
- Candidate Architecture - A proposed structure evaluated against scenarios before committing.
- Architecture Evaluation - Systematic assessment of candidates or the implemented architecture (e.g., via scenarios, ATAM, prototypes).
- Documentation Views - Audience-specific representations (e.g., C4, deployment, runtime views) used to communicate the architecture.

### Microservices terms

- Cross-cutting Concern - A concern that spans multiple services (logging, auth, tracing, config).
- Service Discovery - Mechanism for locating service endpoints dynamically (registry, DNS, sidecars).
- Health Check - Endpoint or probe indicating service liveness/readiness.
- Business Health Check (BHC) - A check validating a business flow outcome, not just service liveness.
- Versioning & Compatibility - Strategies to evolve contracts (e.g., additive changes, tolerant readers, schema compatibility).

### Eventing & contracts

- Schema Registry - Central service storing event schemas and versions for producers/consumers.
- Compatibility Modes - Rules enforced on schema evolution (BACKWARD, FORWARD, FULL).
- Producer - Publishes events.
- Consumer - Subscribes/reads events.
- Event Bus - Managed router for events (e.g., EventBridge) using rules and targets.
- Topic/Stream - Append-only log of events (e.g., Kafka topic) supporting replay via offsets.

### Process & communication

- ADR - Architecture Decision Record - Lightweight doc capturing a key decision, context, options, and consequences.
- Stakeholder - Anyone impacted by the system; informs requirements and constraints.
- Trade-off - An explicit choice balancing competing drivers (e.g., performance vs. cost).
- Solutioneering - the act of creating a solution before understanding the true root of the problem
