# ADR-004: Redis-Backed Queue

- Status: Proposed
- Date: 2026-08-19

## Context

Ingestion and expensive background work must be decoupled, retryable, observable, and scalable.

## Decision

Use a Redis-backed queue for asynchronous work consumed by .NET Worker Services. Redis is not the durable system of record for accepted measurements; durable admission/idempotency state must exist outside the queue.

## Consequences

The implementation needs an outbox/inbox or equivalent failure-safe handoff, bounded retries, dead-letter state, deduplication, backpressure, metrics, and recovery tools. The .NET queue library and exact delivery semantics remain open. This replaces source-specific BullMQ assumptions.

