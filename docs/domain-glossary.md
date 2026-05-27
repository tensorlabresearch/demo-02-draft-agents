# Nearline — Domain Glossary

**Event** — raw inbound message from a Kafka topic. Has a type, a payload (arbitrary JSON),
and a producer timestamp. Events are immutable once consumed from Kafka.

**CanonicalRecord** — the normalised, schema-validated form of an event. This is what
gets written to Postgres. It has a stable schema regardless of which event type produced it.

**Transformer** — the class responsible for converting a specific event type into a
CanonicalRecord. One transformer per event type. Registered in `src/transformers/__init__.py`.

**Schema version** — CanonicalRecords carry a schema version field. Old records are never
migrated; new schema versions are appended. Never change an existing schema version.

**Dead-letter queue (DLQ)** — the Kafka topic where failed events are sent. "DLQ it" means
the event failed transformation and was routed here. These events require human review.

**Lag** — the difference between the Kafka topic offset and the consumer's committed offset.
High lag means the pipeline is falling behind. The SLO is lag < 5,000 events per partition.

**Producer** — an upstream service that writes events to a Kafka topic. Nearline does not
own or modify producers. If a producer sends a malformed event, DLQ it.
