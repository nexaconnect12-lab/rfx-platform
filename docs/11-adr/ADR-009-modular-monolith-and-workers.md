# ADR-009: Modular Monolith and Separately Deployable Workers

- Status: Accepted
- Decision date: 2026-08-20
- Accepted by: Project owner

## Context

RFx Platform must deliver a reliable first vertical slice across device identity, collection sessions, durable batch ingestion, asynchronous processing, measurements, and tenant authorization. These domain boundaries are important, but their detailed models and operational scaling characteristics are still being validated.

Starting with independent microservices would add distributed transactions, network failure modes, service-to-service identity, contract versioning, deployment coordination, tracing, local-development complexity, and additional operational ownership before the project has evidence that these costs are justified.

The system still requires asynchronous workers because admitted ingestion, retries, normalization, dead-letter handling, exports, and later analytics should not be performed inside request-response execution.

## Decision

Build the initial backend as a **Domain-Driven Design modular monolith for the ASP.NET Core API, plus separately deployable .NET Worker Services for asynchronous workloads**.

The initial deployment units are:

1. **RFx API:** one principal ASP.NET Core deployment containing explicit business modules/bounded contexts.
2. **RFx workers:** one or a small number of separately deployable .NET Worker processes, split only where workload, scaling, isolation, or operational ownership justifies it.
3. **Platform dependencies:** PostgreSQL/PostGIS, Redis-backed queue coordination, MinIO where needed, and Keycloak for human identity.

Logical modules do not imply independent services, databases, schemas, repositories, or deployment pipelines.

## Initial API modules

- Identity and Access
- Device Management
- Collection Sessions
- Ingestion
- Measurements
- Later Exports and Reporting

These module names are responsibility boundaries, not frozen solution-folder or assembly names. Detailed structure follows first-slice use-case modeling under ADR-008.

## Boundary rules

- Each module owns its domain and application behavior.
- A module must not directly modify another module's internal aggregate state.
- Cross-module interaction uses explicit application contracts, stable identities, or documented domain/integration events.
- Do not create a shared domain model containing business entities from every module.
- Share only genuinely cross-cutting primitives and technical infrastructure that do not erase domain ownership.
- Domain code remains independent of ASP.NET Core, EF Core, Redis, MinIO, Keycloak SDKs, and worker transport types.
- Database access must preserve module ownership even when modules share one PostgreSQL database.
- Architecture tests should enforce allowed project/module dependencies after initialization.
- Public HTTP contracts are not automatically the contracts used between internal modules.

## Worker rules

- Workers invoke application use cases rather than duplicating domain logic.
- Queue delivery is treated as at least once; consumers must be idempotent.
- Durable PostgreSQL admission/outbox/inbox state protects accepted work; Redis is not the system of record.
- Worker types may scale or deploy independently from the API.
- Separate worker deployments are introduced for demonstrated workload or isolation needs, not for every job type.
- Retry, dead-letter, reconciliation, telemetry, and administrative recovery behavior are explicit and tested.

## Data ownership

The initial modular monolith may use one RFx PostgreSQL database. Shared physical storage does not authorize unrestricted cross-module table access.

- Module-owned writes go through the owning module's application/domain boundary.
- Cross-module reads use explicit query contracts or approved read models where direct relational queries would violate ownership.
- Cross-module invariants must have a documented transaction owner.
- Schema separation may be introduced if it improves ownership and migration safety, but it is not required merely to claim modularity.
- Keycloak retains separate ownership of its identity data.

## Microservice extraction criteria

A module may be proposed for extraction only when evidence demonstrates one or more material needs:

- independently different scaling or resource characteristics;
- repeated requirement for independent release/deployment cadence;
- a stable, separately owned domain boundary with an accountable operating team;
- measurable failure-isolation or availability benefit;
- security, privacy, customer, or regulatory isolation requirement;
- persistent delivery coupling that cannot be solved through better module boundaries;
- a workload requiring materially different infrastructure or runtime characteristics.

Before extraction, document:

- service ownership and on-call responsibility;
- API/event contracts and compatibility policy;
- data ownership and migration/backfill plan;
- consistency model and failure/retry behavior;
- service identity and authorization;
- observability, SLO, deployment, rollback, backup, and disaster-recovery consequences;
- local development and test strategy;
- measured benefit compared with retaining the module.

Extraction requires a new ADR. A code package, module, queue consumer, or separate worker process is not automatically a microservice.

## Likely future candidates

Potential candidates, subject to evidence, include high-volume ingestion processing, export/report generation, advanced analytics, and SmartCare/RTP workloads. They are not pre-approved microservices.

Device, session, ingestion, and measurement ownership should remain within the modular-monolith boundary until their contracts, transaction boundaries, team ownership, and operational characteristics are proven.

## Consequences

The project gains simpler development, transactions, testing, deployment, and operations while retaining explicit domain boundaries and independently scalable background processing.

The team must actively enforce modular boundaries; otherwise the monolith can degrade into tightly coupled code. Architecture tests, code review, module ownership, explicit contracts, and documentation are mandatory safeguards.

This decision complements ADR-001 and ADR-008. It does not authorize implementation before documentation baseline acceptance.

