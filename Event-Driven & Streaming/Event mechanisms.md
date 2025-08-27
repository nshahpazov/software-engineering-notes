

---
title: Event Mechanisms: EventBridge vs Kafka
category: Event-Driven & Streaming
tags: [eventbridge, kafka, events, streaming]
description: Notes comparing AWS EventBridge and Kafka, components, use cases, and trade-offs.
status: notes
---

### AWS EventBridge
AWS Event bridge is an event bus. It sits between producers and consumers and routes events based on rules. It's like a cloud native message router that is less about high-volume streaming (like Kafka) and more about wiring together AWS services and SaaS apps together without glue code.

#### Components
- **Event bus** → like a topic, central pipe where events land.
- **Producer** → calls `PutEvents` or AWS services emit events directly.
- **Rule** → filters events (pattern match on fields).
- **Target** → where events are _pushed_ (Lambda, SQS, Step Functions, API Destination, etc.).
- **Consumers** → don’t exist as first-class entities; you model them by attaching rules → targets.
- **Replay** → not really; you can archive and replay, but it’s not offset-based.
- **Scale** → low-millions of events per second globally, but not designed for raw stream processing (max event size 256 KB, typical integration/eventing workloads).

#### Benefits
 * Tight AWS integration - Native producers (S3, DynamoDB, CodePipeline, etc.) can push events automatically, no plumbing.
 * **SaaS integrations**: Many third-party apps (e.g. Zendesk, Datadog) can publish into your EventBridge bus without custom connectors.
 * **Routing & filtering**: Fine-grained matching on JSON fields (`detail-type`, nested attributes) so consumers only get what they care about.
 * **Fan-out**: One event can trigger multiple consumers (Lambda, Step Functions, SQS, etc.) without you managing delivery.
 * **No infra to manage**: Fully managed, scales transparently, pay per event.
 * **Schema registry/discovery**: EventBridge can automatically infer schemas from incoming events and generate code bindings.

### Common use cases

- **Decoupling AWS service events**: e.g., S3 object created → EventBridge → Lambda for thumbnail generation.
- **Cross-account or cross-region event bus**: share events between org units without VPC peering.
- **SaaS integration**: Stripe webhook → EventBridge → fan out to multiple AWS services.
- **Orchestration**: microservice A emits domain event → multiple downstream services react independently.
- **Audit/monitoring hooks**: central place to capture important system events for logging/alerting.

### Drawbacks / trade-offs

- **Throughput**: designed for “events per second to low thousands.” For heavy firehose data (logs, metrics, telemetry), Kafka/Kinesis is a better fit.    
- **Event size**: 256 KB max per event.
- **At-least-once delivery**: duplicates are possible; your consumers must handle idempotency.
- **Latency**: not real-time streaming fast (tens to hundreds of ms typical).
- **Vendor lock-in**: event format and routing are AWS-specific; you don’t get a portable standard like Kafka’s wire protocol.
- **Limited replay**: you can archive events, but replay isn’t as first-class as in Kafka.

---
So, the rule of thumb:
- **Use EventBridge** when you want a _managed, AWS-native event bus_ for service integration, SaaS hooks, and routing logic without ops overhead.
- **Avoid it** if you need _streaming scale, replay, or open protocols_. That’s when Kafka or Kinesis fit better.


You can connect EventBridge with API destination to connect the events to a any API endpoint, private or public. 


![img](https://docs.aws.amazon.com/images/eventbridge/latest/userguide/images/api-destinations-overview_eventbridge_conceptual.svg)


- You define rules in the event bus which on particular filter (pattern) trigger another AWS service (**target**).
- You can have input transformers which are transformation procedures modifying the content of the event message
- You can apply pattern matching on various event strings
- You can connect an event in the event bus with an API endpoint
- You don't have consumers in the classical PubSub sense, but you have rules which trigger other **API** endpoint **destinations** or AWS services like Firehose, SNS, SQS, Lambda, Step Function, etc.
- EventBridge pushes events to configured targets unlike to the way Kafka works where services pull events; you don’t manage offsets.

### Kafka

#### Components
- **Topic** → a log of events.
- **Producer** → appends events to a topic.
- **Consumer** → client application reads from the log.
- **Consumer group** → manages offsets, partitions, load balancing.
- **Replay** → you can rewind offsets and re-consume history.
- **Scale** → built for high-throughput streams (hundreds of MB/s).
#### Characteristics
* Kafka: high-volume pipelines, streaming analytics, stateful processing.
## When to use Kafka
- **High-throughput, low-latency event streams**: telemetry, logs, clickstreams, IoT data.
- **Event-driven microservices**: producers emit domain events, multiple consumers act independently.
- **Data pipelines**: ingest from many sources, fan out to analytics, ML, or warehousing sinks.
- **Replayability**: you need to reprocess history (bug fix, backfill).
- **Exactly-once pipelines** (with Kafka Streams/Flink integration).

## Drawbacks

- **Operational complexity**: running Kafka clusters is heavy (Zookeeper or now KRaft, tuning, monitoring, upgrades). Managed offerings (Confluent Cloud, MSK) help but cost $.
- **Not real-time push**: consumers pull; latency is low (ms-scale) but not instant push semantics.
- **Storage cost**: retaining high-volume topics for long history can get expensive.
- **Throughput tuning**: partition count, replication factor, producer/consumer configs all matter. 
- **Learning curve**: offset management, idempotence, rebalancing, delivery semantics (at-least-once vs exactly-once) can trip teams.
- **Overkill for simple integration**: if all you need is “S3 event triggers Lambda,” Kafka is heavier than EventBridge/SQS.