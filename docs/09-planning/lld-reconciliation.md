# RFxPro LLD Reconciliation

Status: **Draft — required for baseline acceptance**

## Purpose

RFxPro LLD v1.6 is the priority source input for the first RFxPro delivery. It is not itself an accepted implementation specification. This register records how its material design content maps into the repository source of truth.

RFxPro HLD v2.1 primarily informs future work. It does not expand the first-delivery scope unless a repository requirement, ADR, and roadmap change explicitly adopt that work.

## Reconciliation states

- **Accepted:** approved by an accepted ADR or explicit repository decision.
- **Adopt for baseline:** intended for the first delivery but still subject to baseline review.
- **Adapt:** retain the responsibility or behavior but redesign it for accepted repository decisions.
- **Deferred:** valid future input, outside the first delivery.
- **Rejected:** must not be implemented from the source document.
- **TBD:** unresolved; requires a named decision before its blocking milestone.

## Reconciliation register

| LLD topic | Repository treatment | State | Authority or required action |
|---|---|---|---|
| RFxPro first | Build the RFxPro vertical slice before Lite, SDK, analytics, or SmartCare/RTP | Accepted | Scope and roadmap |
| Kotlin Android collection | Retain Kotlin, Room, WorkManager, foreground collection, and offline-first behavior | Adopt for baseline | Android architecture; ADR-006 remains proposed |
| Android package tree | Treat the source package tree as a responsibility map, not a fixed package/API contract | Adapt | Approve exact modules after ADR-006 and toolchain decisions |
| Node.js/Express/Fastify or Kotlin/Ktor backend | Replace with ASP.NET Core APIs and .NET Worker Services | Rejected | ADR-001, accepted 2026-08-20 |
| Redis + BullMQ | Retain Redis-backed asynchronous coordination; replace BullMQ with a selected .NET-compatible queue/job library | Adapt | ADR-004 and decision DR-008 |
| Direct Redis handoff | Redis must not be the sole durable copy of accepted work | Rejected | Backend architecture; select outbox/inbox or equivalent in DR-009 |
| PostgreSQL 16/PostGIS | Retain as the operational and geospatial system of record | Adopt for baseline | ADR-003 remains proposed |
| Source MVP DDL | Treat as domain discovery only; do not generate migrations from it | Adapt | Data architecture and canonical data model decisions |
| MinIO | Retain for large immutable inputs/outputs when object storage is required | Adopt for baseline | ADR-005 remains proposed |
| Locally signed dashboard JWT / `JWT_SECRET` | Replace with Keycloak-issued human tokens and API validation | Rejected | Authentication architecture; ADR-002 remains proposed |
| Device API token | RFx backend owns device identity; credential format, bootstrap, rotation, revocation, and recovery remain unresolved | Adapt | DR-006 |
| API token in Room/config entity | Store credentials using an approved OS-backed secure mechanism, never Room | Rejected | Android and authentication architecture |
| Client-created sessions and batches | Retain stable client-generated identifiers; authoritative session-ID and acknowledgement behavior remain unresolved | Adapt | API principles; DR-003 and DR-004 |
| Measurement upload endpoints and examples | Use as workflow input only; OpenAPI will define paths, schemas, limits, errors, and versioning | Adapt | DR-003 through DR-005 |
| Idempotent retry | Mandatory first-delivery invariant | Adopt for baseline | Scope, API principles, backend architecture, and testing strategy |
| Marking local rows synchronized | Permit only after the approved durable server acknowledgement; cleanup/retention behavior remains TBD | Adapt | DR-004 and DR-010 |
| Cell-reference import/matching | Subsequent RFxPro increment, not the first vertical slice | Deferred | Scope and roadmap Phase 3 |
| Speed tests | Subsequent RFxPro increment | Deferred | Scope and roadmap Phase 3 |
| Dashboard, GeoServer, MapProxy, and reports | Subsequent delivery after the ingestion slice | Deferred | Roadmap Phase 4 |
| RFx Lite | Later product after proven RFxPro foundations | Deferred | Scope and roadmap later phases |
| RFx SDK and consent surface | Partnership-gated future capability requiring its own contract and review | Deferred | Scope and roadmap later phases |
| Busy-hour/advanced analytics | Future work; do not prebuild schema solely for speculative analytics | Deferred | Scope and roadmap later phases |
| SmartCare/RTP | Future enterprise work requiring separate charter, hardware, privacy, and operations decisions | Deferred | Scope and roadmap later phases |
| Diag/L3/root capabilities | Separate deferred track requiring access, legality, device, security, and product approval | Deferred | Scope and Android architecture |
| Call and OTT tests | Not a first-slice requirement; prototype separately only after product approval | Deferred | Scope open issue and roadmap |
| Source code samples | Illustrative only; they are not repository code, contracts, security guidance, or implementation authorization | Rejected | Engineering agreement and baseline gate |

## Completion rule

Baseline acceptance does not require every future item to be designed. It requires every first-delivery item to be either accepted or explicitly assigned a decision ID, owner role, and blocking milestone. No source LLD statement may silently become an implementation contract.

