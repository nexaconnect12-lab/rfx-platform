# First-Slice Logical Data Model

Status: **Proposed — not approved DDL or migration content**

## Entities

This logical model supports the Domain-Driven Design direction in ADR-008. Tables and EF Core mappings do not define aggregate boundaries by themselves; aggregate boundaries are established by invariants and transaction needs.

| Entity | Purpose | Required relationships/invariants |
|---|---|---|
| Tenant | Root business/authorization boundary | Immutable ID; owns projects, devices, memberships, sessions |
| Project | Groups authorized field work | Belongs to one tenant; status controls new session creation |
| Device | RFx-managed application installation identity | Belongs to one tenant; credential lifecycle and capability recorded |
| Session | Client-created RFxPro collection session | Client UUID; belongs to tenant/project/device; upload and processing states separated |
| SessionStateHistory | Auditable session state transitions | Append-only transition facts with actor/correlation/time |
| IngestionBatch | Durable idempotency/admission and processing record | Unique tenant/device/session/batch identity; immutable payload digest after admission |
| BatchAttempt | Worker attempt and outcome | Append-only attempt number, timing, safe error classification |
| Measurement | Normalized RF/location fact | Belongs to one session and originating batch; capture time/provenance retained |
| AuditEvent | Sensitive administrative/security action | Tenant scope, actor, action, target, result, correlation, timestamp |
| ObjectMetadata | Optional reference to MinIO payload/export | Tenant ownership, checksum, media type, size, lifecycle; bucket/key not public authorization |

## Recommended invariants

- Tenant scope is explicit on authorization-critical aggregate roots and enforced by constraints/query policy.
- Public session ID is the client UUID recommended in DR-003.
- Ingestion uniqueness is `(tenant_id, device_id, session_id, batch_id)`.
- An admitted batch's payload digest and scope are immutable.
- Identical retry returns the existing batch; same identity with a different digest is a conflict.
- Measurements are idempotently associated with the originating batch and observation ID.
- Redis contains work coordination, never the sole accepted payload or idempotency fact.
- Session upload state and processing state are different concepts.
- Raw facts and derived values are stored separately.
- Keycloak owns its own schema/database and migrations.

## DDD modeling guidance

- Treat `Device`, `CollectionSession`, and `IngestionBatch` as initial aggregate candidates subject to use-case validation.
- Keep high-volume measurements outside a single session object graph; process and persist them through admitted batch scope.
- Do not create database foreign-key navigation behavior that permits bypassing tenant/project authorization.
- Use database constraints as defense in depth for domain invariants, tenancy, idempotency, and immutable admitted-batch identity.
- Keep EF Core persistence types/configuration from leaking into the domain model where that coupling would distort domain behavior.

## Durable handoff recommendation

Use a PostgreSQL transactional outbox:

1. API transaction writes ingestion batch, durable payload/reference, and outbox message.
2. A .NET dispatcher publishes the outbox message to the selected Redis-backed queue.
3. A .NET worker consumes with at-least-once delivery and uses inbox/attempt/idempotency state.
4. Measurement commit and successful batch transition occur transactionally where practical.
5. Reconciliation detects admitted batches/outbox records that are not progressing.

The exact queue library remains DR-008. This recommendation resolves the architectural failure window without treating Redis as the system of record.

## Physical design decisions deferred until query/volume evidence

- primary-key storage types beyond public UUIDs;
- table/index names and EF Core mappings;
- measurement partitioning and archival;
- JSONB usage;
- spatial/time/operator indexes;
- raw admitted payload in PostgreSQL versus immutable MinIO object plus transactional metadata;
- retention periods and deletion/anonymization mechanics.

No source HLD/LLD DDL may be converted into migrations until this logical model, measurement dictionary, privacy policy, and OpenAPI contract are accepted.
