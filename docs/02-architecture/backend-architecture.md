# Backend Architecture

Status: **Draft — awaiting baseline acceptance**

## Accepted backend decision

Per accepted ADR-001, use ASP.NET Core as the main backend system and .NET Worker Services for asynchronous/background work. Start with clear modules and one principal API deployment rather than a broad microservice estate.

Per accepted ADR-008, use Domain-Driven Design as the principal backend design approach. Per accepted ADR-009, model those responsibilities inside an initial modular monolith/API plus separately deployable .NET workers. Microservice extraction requires evidence and a new ADR.

RFxPro LLD v1.6 is the priority source input for first-delivery behavior and domain detail, but its Node.js, Express/Fastify, Kotlin/Ktor, BullMQ, and locally signed dashboard-JWT examples are superseded. They must be translated into the accepted .NET and Keycloak architecture rather than copied into implementation.

## Logical layers

- **API/transport:** HTTP, authentication scheme selection, request validation, rate/size limits, problem responses.
- **Application:** use cases, authorization policies, transactions, idempotency coordination.
- **Domain:** aggregates, entities, value objects, domain services/events, sessions, devices, ingestion batches, measurement rules, processing states, exports, and tenant rules.
- **Infrastructure:** PostgreSQL/PostGIS, Redis queue, MinIO, Keycloak validation, telemetry.
- **Workers:** durable job consumption, parsing/transformation, dead-letter handling, derived data, export/report work.

Dependencies point inward; domain logic must not depend directly on controllers, EF Core, Redis, MinIO, or Keycloak SDK types.

Initial candidate modules are Identity and Access, Device Management, Collection Sessions, Ingestion, Measurements, and later Exports/Reporting. These logical boundaries do not imply separate microservices or databases. Detailed modeling follows ADR-008.

Modules own their writes and business behavior. Cross-module interaction uses explicit application contracts, stable identities, or documented events. Sharing one RFx PostgreSQL database does not permit unrestricted access to another module's internal tables or aggregate state. Architecture tests must enforce dependency direction after initialization.

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

- Pin the supported .NET SDK/runtime version.
- Choose EF Core/data-access and module transaction boundaries, queue implementation, outbox/inbox implementation, and job framework.
- Approve the initial aggregate boundaries and ubiquitous-language glossary through first-slice use-case modeling.
- Approve exact session state machine and recovery/admin operations.
- Decide synchronous versus asynchronous ingestion thresholds and response contracts.
- Define service SLOs, rate limits, health/readiness semantics, and telemetry platform.
