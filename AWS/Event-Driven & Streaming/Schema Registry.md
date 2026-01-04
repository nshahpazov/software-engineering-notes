
---
title: Schema Registry Notes
category: Event-Driven & Streaming
tags: [kafka, schema-registry, compatibility, events]
description: Notes on using a schema registry for Kafka and event versioning.
status: notes
---

- The registry enforces **compatibility rules** (BACKWARD, FORWARD, FULL).
- Each Kafka message carries a small ID that points to the exact writer schema in the registry.
- No more “which version of the Pydantic class was deployed when this event was produced?”
- If someone tries to register a breaking change (e.g. remove/rename a field under BACKWARD), the registry rejects it.
- Every change creates a new schema version.