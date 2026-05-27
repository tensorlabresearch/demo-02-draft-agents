# Nearline — Testing Style

## Structure
Tests live in `tests/`. Unit tests mirror the `src/` structure.
Integration tests that require Kafka or Postgres live in `tests/integration/`
and are excluded from the default test run.

Default run: `pytest tests/ -x --tb=short --ignore=tests/integration`
Integration run: `pytest tests/integration/ -x --tb=short` (requires docker-compose up)

## What to test
- Transformers: test with a known raw event dict. Assert the returned CanonicalRecord fields exactly.
- Ingesters: test the parsing and routing logic only. Mock the Kafka consumer.
- Writers: test the repository layer with a real SQLite database (not Postgres) in tests.

## Naming
Pattern: `test_<thing>_<condition>_<expected_outcome>`
Example: `test_transformer_missing_payload_field_raises_dlq_error`

## What not to test
- Do not test Kafka connectivity in unit tests.
- Do not test Postgres connectivity in unit tests. Use the SQLite repo fixture.
- Do not write tests that depend on test ordering or global state.

## Fixtures
`conftest.py` at the root of `tests/` provides:
- `sample_event(type)` — returns a minimal valid raw event dict for the given type
- `sqlite_repo` — an in-memory SQLite-backed repository for writer tests
