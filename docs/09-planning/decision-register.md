# Planning Decision Register

Status: **Draft — required for baseline acceptance**

## Purpose

This register consolidates decisions that affect baseline acceptance and the first RFxPro vertical slice. `TBD` means the project has not approved the answer. Owner roles must be replaced with named people in the acceptance record or project tracker.

## Decisions

| ID | Decision | Status | Owner role | Blocks | Evidence/update target |
|---|---|---|---|---|---|
| DR-001 | Main backend platform: ASP.NET Core APIs and .NET Worker Services | Accepted | Project owner | Resolved | ADR-001 and backend architecture |
| DR-002 | Exact first-slice boundary and acceptance evidence | Proposed in repository | Product owner + technical lead | Baseline acceptance | Vertical-slice acceptance document |
| DR-003 | Whether the client-generated session UUID is authoritative end to end | TBD | API/backend lead + Android lead | OpenAPI and data model | API principles, OpenAPI, data architecture |
| DR-004 | Batch identity, durable acknowledgement, duplicate response, and safe Android cleanup semantics | TBD | API/backend lead + Android lead | OpenAPI and vertical slice | API principles, Android/backend architecture, tests |
| DR-005 | Canonical measurement fields, units, ranges, nullability, timestamp precision, SRID, provenance, and quality flags | TBD | RF/domain owner + data lead | OpenAPI and schema | Measurement dictionary, API, data architecture |
| DR-006 | Device credential format, bootstrap, secure storage, rotation, revocation, and recovery | TBD | Security owner + backend lead + Android lead | Device registration implementation | Authentication architecture and threat model |
| DR-007 | Initial tenant model and minimum role-to-permission matrix | TBD | Product owner + security owner | Domain schema and authorization contracts | Authentication architecture and data model |
| DR-008 | .NET-compatible Redis queue/job library and delivery semantics | TBD | Backend/platform lead | Worker foundation | ADR-004 supersession/addendum if decision changes its scope |
| DR-009 | Durable API-to-worker handoff: outbox/inbox or equivalent | TBD | Backend/data lead | Ingestion implementation | Backend architecture and data model |
| DR-010 | Android batching, retry/backoff, pending-data retention, quota, and cleanup policy | TBD | Android lead + product owner | Sync implementation and tests | Scope, Android architecture, API contract |
| DR-011 | Supported Android/OEM/API pilot device matrix and canonical capability normalization | TBD | RF/domain owner + Android lead | Field acceptance | Android architecture and testing strategy |
| DR-012 | Exact .NET, Android, Node/pnpm, and container versions | TBD | Platform lead + component leads | Project initialization | Environment setup |
| DR-013 | Persistence library and data-access/module boundaries | TBD | Backend/data lead | Backend foundation | Backend architecture; ADR if architectural consequences are material |
| DR-014 | Privacy classification, consent boundary, retention, deletion, export audit, and test-data policy | TBD | Privacy/security owner + product owner | Pilot approval | Scope, data architecture, testing, operations |
| DR-015 | Minimum service SLOs, capacity assumptions, rate/payload limits, RPO, and RTO | TBD | Product owner + platform lead | Production/pilot readiness; only payload limits block initial contracts | Charter, API, testing, operations |
| DR-016 | Monitoring, logs, traces, metrics, alerting, health/readiness, and sensitive-data redaction platform | TBD | Platform lead + security owner | Phase 1 foundation | Backend architecture and environment/operations docs |
| DR-017 | Named baseline approver(s), accepted commit, date, and exceptions | TBD | Project owner | Baseline acceptance | Engineering agreement acceptance record |

## Decision protocol

For each `TBD` decision:

1. Record candidate options and material tradeoffs in the affected document or a new ADR.
2. Identify the named decision maker and review participants.
3. Record the decision and date; do not infer approval from implementation.
4. Update all affected requirements, contracts, architecture, data, security, tests, environment, and roadmap documents together.
5. Use a superseding ADR when changing an accepted ADR.

## Baseline blocking policy

DR-002 through DR-007, DR-009 through DR-014, and DR-017 must be resolved or explicitly accepted as a documented constraint before the baseline is accepted. DR-008, DR-015, and DR-016 may be split into baseline-critical and later operational portions, but any part required to define contracts or initialize the project must be resolved before that work begins.

