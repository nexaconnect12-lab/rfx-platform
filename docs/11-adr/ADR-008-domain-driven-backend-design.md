# ADR-008: Domain-Driven Backend Design

- Status: Accepted
- Decision date: 2026-08-20
- Accepted by: Project owner

## Context

The RFx backend contains domain behavior that extends beyond HTTP endpoints and database CRUD: tenant and project authorization, device credential lifecycle, offline session admission, idempotent batch handling, processing state transitions, measurement provenance, exports, and audit requirements.

The backend needs a design approach that keeps these rules explicit and testable while preserving the accepted ASP.NET Core modular-monolith direction. The project must avoid coupling business behavior directly to controllers, Entity Framework Core, Redis, MinIO, Keycloak, or transport-specific models.

## Decision

Use **Domain-Driven Design (DDD)** as the principal backend design approach for ASP.NET Core APIs and .NET Worker Services.

Organize backend behavior into explicit bounded contexts or modules based on business responsibility. The initial candidate contexts are:

- **Identity and Access:** RFx user profiles, tenant memberships, authorization policy inputs, and audit integration; Keycloak remains the human identity provider.
- **Device Management:** device registration, credential lifecycle, status, capability, assignment, rotation, and revocation.
- **Collection Sessions:** project-scoped session lifecycle, upload completion, and processing state.
- **Ingestion:** durable batch admission, idempotency, payload identity, outbox/inbox coordination, attempts, rejection, and dead-letter behavior.
- **Measurements:** canonical RF/location observations, provenance, validation/normalization, and geospatial persistence.
- **Exports and Reporting:** later authorized export/report lifecycle and object metadata.

These are logical boundaries. They do not imply one service, database, schema, repository, or deployment per context.

## Layer responsibilities

- **Domain:** aggregates, entities, value objects, domain services, invariants, domain events, and domain errors expressed without infrastructure dependencies.
- **Application:** use cases, commands/queries where useful, authorization orchestration, transactions, idempotency coordination, ports/interfaces, and result contracts.
- **Infrastructure:** EF Core or selected persistence implementation, PostgreSQL/PostGIS, Redis queue integration, MinIO, Keycloak adapters, telemetry, clock/identifier implementations, and external services.
- **API/transport:** HTTP routing, authentication scheme selection, request/response mapping, validation of transport shape, versioning, rate/size enforcement, and problem-details translation.
- **Workers:** message transport handling and invocation of application use cases; workers do not become an alternate home for domain rules.

Dependencies point inward. Domain projects/modules must not reference ASP.NET Core controllers, EF Core, Redis clients, MinIO clients, Keycloak SDK types, or OpenAPI-generated transport types.

## Modeling rules

- Use the terminology defined in accepted project documentation and the measurement dictionary as the ubiquitous language.
- Put invariants in the aggregate/entity/value object that owns them rather than duplicating them across controllers and workers.
- Keep aggregates small and transactionally meaningful. Do not load a complete session's measurement history merely to admit a batch.
- Reference other aggregates by stable identity instead of creating large object graphs.
- Distinguish domain validation from transport validation and infrastructure failures.
- Keep persistence models and API contracts separate from domain models when their responsibilities differ.
- Publish domain events only for meaningful completed domain facts. Use the transactional outbox for reliable integration/event delivery where required.
- Treat timestamps, identifiers, tenant/project scope, measurement units, batch digest, and processing states as explicit types/value concepts where doing so prevents invalid combinations.
- Maintain deterministic, side-effect-free domain behavior where practical; supply clock, identity, storage, and external operations through application/infrastructure boundaries.

## Pragmatic constraints

DDD adoption does **not** automatically authorize:

- a microservice per bounded context;
- separate databases or schemas per module;
- event sourcing;
- CQRS infrastructure for every use case;
- a message broker for internal in-process communication;
- a generic repository or unit-of-work abstraction over every entity;
- unnecessary factories, specifications, domain events, or abstraction layers;
- speculative modeling of RFx Lite, SDK, analytics, SmartCare/RTP, or Diag domains.

Use direct, testable code when the behavior is simple. Introduce patterns only when they protect a real invariant, boundary, compatibility requirement, or test seam.

## Initial aggregate candidates

The following are candidates to validate during detailed modeling, not frozen class designs:

- `Device` for lifecycle, capability, assignments, and credential status metadata;
- `CollectionSession` for session/upload/processing lifecycle;
- `IngestionBatch` for durable admission, payload identity, idempotency, and processing outcome;
- `Project` or membership policy boundary where assignment rules require domain behavior;
- `Export` in the later dashboard/reporting phase.

Measurements are high-volume facts and should not form one unbounded in-memory aggregate under a session. Their integrity is enforced through the admitted batch/session scope, canonical value rules, database constraints, and idempotent processing.

## Testing consequences

- Unit-test aggregate invariants, value objects, policies, and state transitions without infrastructure.
- Test application use cases with controlled ports and explicit authorization context.
- Integration-test EF Core/PostgreSQL/PostGIS mappings, constraints, transactions, outbox/inbox behavior, queue delivery, Keycloak token mapping, and MinIO metadata flows.
- Contract-test API DTO/domain mapping and stable problem codes.
- End-to-end tests remain required for offline retry, ambiguous acknowledgements, duplicate delivery, authorization isolation, and traceability.

## Consequences

The backend gains explicit domain boundaries and testable business rules at the cost of additional modeling discipline and mapping between transport, application, domain, and persistence concerns.

Module boundaries and aggregate designs must be reviewed against real first-slice use cases. Exact solution/project structure, EF Core boundaries, command/query tooling, mediator use, validation library, and mapping library remain separate implementation decisions. A library must not dictate the domain model.

This decision complements ADR-001. ASP.NET Core and .NET Worker Services remain the backend platform, initially deployed as a limited modular monolith/API plus workers.

