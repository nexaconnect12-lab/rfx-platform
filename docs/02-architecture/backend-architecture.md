# Backend Architecture

Status: **Draft — awaiting baseline acceptance**

## Decision direction

Use ASP.NET Core for the API and .NET Worker Services for asynchronous/background work. Start with clear modules and one principal API deployment rather than a broad microservice estate.

## Logical layers

- **API/transport:** HTTP, authentication scheme selection, request validation, rate/size limits, problem responses.
- **Application:** use cases, authorization policies, transactions, idempotency coordination.
- **Domain:** sessions, devices, ingestion batches, measurements, processing states, exports, and tenant rules.
- **Infrastructure:** PostgreSQL/PostGIS, Redis queue, MinIO, Keycloak validation, telemetry.
- **Workers:** durable job consumption, parsing/transformation, dead-letter handling, derived data, export/report work.

Dependencies point inward; domain logic must not depend directly on controllers, EF Core, Redis, MinIO, or Keycloak SDK types.

## Ingestion lifecycle

1. Authenticate device and authorize the target session/tenant.
2. Validate envelope, schema version, size, and client-generated `batchId`.
3. Atomically record batch admission/idempotency state.
4. Enqueue a reference to durable work; do not rely on Redis as the sole copy of accepted measurements.
5. Return an accepted/duplicate result with a traceable batch status.
6. Worker processes and commits measurements idempotently.
7. Failures retry within policy and then enter a queryable dead-letter state.

## Session states

Provisional model: `OPEN`, `UPLOAD_COMPLETE`, `PROCESSING`, `PROCESSED`, `FAILED`. Calling `complete` means the client has finished uploading; it must not falsely imply queued work has finished.

## Open issues

- Choose EF Core/data-access boundaries, queue implementation, outbox/inbox pattern, and job framework.
- Approve exact session state machine and recovery/admin operations.
- Decide synchronous versus asynchronous ingestion thresholds and response contracts.
- Define service SLOs, rate limits, health/readiness semantics, and telemetry platform.

