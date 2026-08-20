# Planning Decision Register

Status: **Draft — required for baseline acceptance**

## Purpose

This register consolidates decisions that affect baseline acceptance and the first RFxPro vertical slice. `TBD` means the project has not approved the answer. Owner roles must be replaced with named people in the acceptance record or project tracker.

## Decisions

| ID | Decision | Status | Owner role | Blocks | Evidence/update target |
|---|---|---|---|---|---|
| DR-001 | Main backend platform: ASP.NET Core APIs and .NET Worker Services | Accepted | Project owner | Resolved | ADR-001 and backend architecture |
| DR-002 | Exact first-slice boundary and acceptance evidence | Recommended proposal documented | Product owner + technical lead | Baseline acceptance | Vertical-slice acceptance document |
| DR-003 | Client-generated session UUID is authoritative end to end | Recommended proposal documented | API/backend lead + Android lead | Baseline acceptance | First-slice API contract, OpenAPI, logical data model |
| DR-004 | Batch identity, durable acknowledgement, duplicate response, and safe Android cleanup semantics | Recommended proposal documented | API/backend lead + Android lead | Baseline acceptance | First-slice API contract and OpenAPI |
| DR-005 | Canonical measurement fields, units, nullability, timestamps, SRID, provenance, and quality flags | Partial proposal; RF rules TBD | RF/domain owner + data lead | Baseline acceptance | Measurement dictionary and OpenAPI |
| DR-006 | Opaque device credential, bootstrap, secure storage, rotation, revocation, and recovery | Recommended proposal documented; policy values TBD | Security owner + backend lead + Android lead | Baseline acceptance | Device identity and threat model |
| DR-007 | Initial tenant model and minimum role-to-permission matrix | Recommended proposal documented | Product owner + security owner | Baseline acceptance | Tenancy/authorization model and logical data model |
| DR-008 | .NET-compatible Redis queue/job library and delivery semantics | Options/criteria documented; library TBD | Backend/platform lead | Worker foundation | ADR-004 and toolchain record |
| DR-009 | PostgreSQL transactional outbox with idempotent worker/inbox behavior | Recommended proposal documented | Backend/data lead | Baseline acceptance | Logical data model and backend architecture |
| DR-010 | Android batching, retry/backoff, pending-data retention, quota, and cleanup policy | Acknowledgement invariant proposed; numeric policies TBD | Android lead + product owner | Baseline acceptance | First-slice API contract and privacy lifecycle |
| DR-011 | Supported Android/OEM/API pilot device matrix and canonical capability normalization | TBD | RF/domain owner + Android lead | Field acceptance | Android architecture and testing strategy |
| DR-012 | Exact .NET, Android, Node/pnpm, and container versions | Selection policy documented; versions TBD | Platform lead + component leads | Project initialization | Toolchain selection record |
| DR-013 | Persistence library and data-access/module boundaries | Logical boundaries documented; library TBD | Backend/data lead | Backend foundation | Backend architecture and logical data model |
| DR-014 | Privacy classification, consent boundary, retention, deletion, export audit, and test-data policy | Classification/minimization proposed; retention approvals TBD | Privacy/security owner + product owner | Baseline/pilot approval | Privacy and data lifecycle plan |
| DR-015 | Minimum service SLOs, capacity assumptions, rate/payload limits, RPO, and RTO | Decision fields documented; numeric targets TBD | Product owner + platform lead | Contracts and pilot readiness | API contract and operational readiness plan |
| DR-016 | Monitoring, logs, traces, metrics, alerting, health/readiness, and sensitive-data redaction platform | Minimum signals/semantics proposed; platform/thresholds TBD | Platform lead + security owner | Phase 1 foundation | Operational readiness plan and threat model |
| DR-017 | Named baseline approver(s), accepted commit, date, and exceptions | TBD | Project owner | Baseline acceptance | Engineering agreement acceptance record |
| DR-018 | Dashboard design system: React + TypeScript + Ant Design; React Leaflet/Leaflet for maps | Accepted | Project owner | Resolved; implementation remains Phase 4 | ADR-007 and toolchain selection record |
| DR-019 | Backend design approach: pragmatic Domain-Driven Design within ASP.NET Core modular monolith/API plus .NET workers | Accepted | Project owner | Resolved | ADR-008 and backend architecture |
| DR-020 | Initial deployment architecture: ASP.NET Core modular monolith plus separately deployable .NET workers; evidence-based microservice extraction only | Accepted | Project owner | Resolved | ADR-009 and backend/system architecture |

## Decision protocol

For each unresolved or recommended-but-unaccepted decision:

1. Record candidate options and material tradeoffs in the affected document or a new ADR.
2. Identify the named decision maker and review participants.
3. Record the decision and date; do not infer approval from implementation.
4. Update all affected requirements, contracts, architecture, data, security, tests, environment, and roadmap documents together.
5. Use a superseding ADR when changing an accepted ADR.

## Baseline blocking policy

DR-002 through DR-007, DR-009 through DR-014, and DR-017 must be resolved or explicitly accepted as a documented constraint before the baseline is accepted. DR-008, DR-015, and DR-016 may be split into baseline-critical and later operational portions, but any part required to define contracts or initialize the project must be resolved before that work begins.
