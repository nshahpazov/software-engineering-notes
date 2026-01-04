---
title: Business Health Check Example
category: Operations & Observability
tags: [aws, eventbridge, lambda, monitoring, health-check]
description: Example architecture for a scheduled supervisor invoking an API and tracking outcomes.
status: notes
---

# Business health check example

### ASCII diagram for the business health check example:


                        ┌────────────────────┐
                        │   EventBridge Rule │
                        │ (every 5 minutes)  │
                        └────────┬───────────┘
                                 │
                                 ▼
                       ┌──────────────────────┐
                       │  HealthCheck Lambda  │◄──┐
                       │  (Supervisor)        │   │
                       └────────┬─────────────┘   │
                                │                 │
                                ▼                 │
                  ┌─────────────────────────────┐ │
                  │  API Gateway                │ │
                  │  /process-order (POST)      │ │
                  └────────┬────────────────────┘ │
                           │                      │
                           ▼                      │
                ┌──────────────────────────┐      │
                │ Dummy Lambda (Handler)   │      │
                │ Simulates business logic │      │
                └──────────────────────────┘      │
                           │                      │
                           ▼                      │
               ┌────────────────────────────┐     │
               │ CloudWatch Logs (metrics,  │     │
               │ duration, success/failure) │◄────┘
               └────────────────────────────┘



### Mermaid diagram for the business health check example:


```mermaid
flowchart TD
  EventBridge["EventBridge Rule<br />(every 5 minutes)"]
  HealthCheck["HealthCheck Lambda<br />(Supervisor)"]
  APIGW["API Gateway<br />/process-order (POST)"]
  DummyLambda["Dummy Lambda<br />Simulated Business Logic"]
  CloudWatch["CloudWatch Logs<br />Duration & Success/Failure"]

  EventBridge --> HealthCheck
  HealthCheck --> APIGW
  APIGW --> DummyLambda
  DummyLambda --> CloudWatch
  HealthCheck --> CloudWatch
```