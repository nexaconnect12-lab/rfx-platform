# Delivery Ownership and Milestone Plan

Status: **Draft — names, dates, and estimates require project-owner approval**

## Required named roles

| Role | Accountable decisions/deliverables | Named owner |
|---|---|---|
| Project/product owner | Scope, priority, commercial/pilot approval, acceptance authority | TBD |
| Technical lead | Cross-component architecture and baseline coherence | TBD |
| RF/domain owner | Measurement dictionary, device validation, tolerances | TBD |
| Android lead | Capture, Room, WorkManager, device capability matrix | TBD |
| Backend/API lead | OpenAPI, authorization enforcement, ingestion/application design | TBD |
| Data lead | Logical/physical model, PostGIS, migrations, backup integrity | TBD |
| Platform/operations lead | Environments, CI, secrets, telemetry, recovery | TBD |
| Security/privacy owner | Threat model, credentials, tenancy review, privacy lifecycle | TBD |
| QA/test owner | Test mapping, evidence, release recommendation | TBD |
| Pilot/customer owner | Pilot access, devices, operator coordination, field schedule | TBD |

One person may hold multiple roles, but accountability must be explicit. Reviewers should be independent where security/privacy or release risk warrants it.

## Milestone gates

| Milestone | Entry | Exit evidence | Owner | Target/estimate |
|---|---|---|---|---|
| M0 Baseline review | Planning package complete | Required decisions accepted/deferred, checklist signed, commit recorded | Project owner | TBD |
| M1 Contracts and foundation | M0 accepted | Pinned toolchains, reviewed OpenAPI/data/threat model, CI/environment design approved | Technical lead | TBD |
| M2 Device-local capture | M1 complete | Approved device captures and persists canonical observations through recovery scenarios | Android + RF owners | TBD |
| M3 Durable ingestion | M1 complete | Authorized durable admission, outbox/worker processing, PostGIS verification | Backend + data owners | TBD |
| M4 End-to-end vertical slice | M2 and M3 complete | VS-01 through VS-12 evidence accepted for required scenarios | QA + technical lead | TBD |
| M5 Pilot readiness | M4 complete | Privacy/security/operations gates, restore/revocation/rollback, pilot plan approved | Project + security + platform owners | TBD |
| M6 Pilot exit | Approved pilot executed | Exit criteria, defects/risks, product decision, roadmap update | Project/pilot owner | TBD |

## Estimation protocol

Estimates are produced only after M0 decisions define the work. Each milestone estimate records assumptions, confidence, dependencies, staffing, review time, device/field availability, and contingency. Do not use source-deck phase labels as delivery commitments.

## Pilot entry criteria

- approved tenant/project and device list;
- signed data-purpose/privacy/retention decision;
- supported app/backend builds and rollback plan;
- tested revocation, batch reconciliation, monitoring, backup/restore;
- named support and incident contacts;
- agreed test routes/scenarios and reference method;
- known limitations communicated to participants/customer.

## Pilot exit criteria

- required vertical-slice scenarios pass with retained evidence;
- RF/domain comparison results are reviewed against approved tolerances;
- no unresolved critical security, data-loss, duplication, or tenant-isolation defect;
- operational incidents and recovery results are documented;
- product owner records proceed, remediate/repeat, or stop decision;
- roadmap, risks, decisions, and affected documentation are updated.

