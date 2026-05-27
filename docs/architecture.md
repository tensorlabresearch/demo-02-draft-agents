# Nearline — Architecture

Nearline is a data pipeline service that ingests events from upstream producers,
transforms them into canonical records, and writes them to a durable store.
It is written in Python 3.12 and runs as a set of long-lived worker processes.

## Core components

**Ingester** — reads raw events from a Kafka topic. One ingester per topic.
Ingesters are stateless; they do not persist anything locally.

**Transformer** — applies a schema to each raw event and produces a CanonicalRecord.
Transformers are pluggable: each event type has a registered transformer class.
New event types require a new transformer — do not modify existing ones.

**Writer** — batches CanonicalRecords and writes to Postgres via the repository layer.
All database access goes through repository classes in `src/repos/`.
Never write SQL directly in transformers or ingesters.

**Dead-letter queue (DLQ)** — events that fail transformation are written to the DLQ topic.
DLQ events must never be silently dropped. They must always be written or raise.

## Build and test
pip install -r requirements.txt
pytest tests/ -x --tb=short
Pre-commit: ruff check . && mypy src/

## Key invariants
- A CanonicalRecord is immutable once written. Never update, only append.
- Transformer output must be idempotent: same input always produces same output.
- The ingester must never block the Kafka consumer loop. Slow work goes to a thread pool.
