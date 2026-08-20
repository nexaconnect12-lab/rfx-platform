# Implementation Roadmap

Status: **Draft — awaiting baseline acceptance**

## Gate 0 — accept the baseline

Review every document and ADR, resolve or explicitly defer blocking open issues, name approvers, and record acceptance. **No application source generation before this gate passes.**

Resolved decision: ASP.NET Core is the main backend system and .NET Worker Services perform asynchronous/background processing (ADR-001, accepted 2026-08-20). Backend runtime selection no longer blocks the baseline; .NET version and implementation-library choices remain open.

Gate artifacts: LLD reconciliation, planning decision register, vertical-slice acceptance specification, and documentation baseline acceptance checklist.

Milestone ownership, entry/exit evidence, and required role assignments are defined in the [Delivery Ownership and Milestone Plan](../09-planning/delivery-ownership-plan.md).

## Phase 1 — contracts and executable foundation

After acceptance: establish repository structure, pinned toolchains, Docker Compose infrastructure, initial threat/data model, OpenAPI skeleton, migrations, observability conventions, and CI quality gates.

## Phase 2 — RFxPro vertical slice

Device registration → session UUID → Open/Developer RF/GPS capture → Room → WorkManager batch sync → ASP.NET Core admission → Redis queue → .NET Worker → PostGIS. Demonstrate offline continuation, safe retry, idempotency, and traceability.

## Phase 3 — usable RFxPro

Session UI/history, maps, NEI/cell reference import and matching, speed testing, exports, field-device validation, recovery/admin workflows, and performance hardening.

## Phase 4 — dashboard and geospatial delivery

Keycloak login, scoped query APIs, React/Leaflet dashboard, GeoServer publication, MapProxy/OSM reuse, reports, audits, and operational readiness.

## Phase 5 — production readiness

Security/privacy review, tenancy tests, SLOs, monitoring/alerting, backups/restores, retention/deletion, capacity tests, runbooks, and pilot rollout.

## Later phases

RFx Lite → RFx SDK → advanced analytics → RFx SmartCare/RTP. Each requires its own charter delta, risks, consent/security review, ADRs, and acceptance criteria. Diag remains deferred until separately approved.

## Tracking rule

Every implementation change updates this roadmap’s status/dependencies and all affected requirements, architecture, contracts, schemas, ADRs, and tests before completion.

## Open issues

- Replace owner roles in the planning decision register with named people.
- Define estimates, milestones, pilot entry/exit criteria, and release governance.
- Decide which open architecture/contract issues block Phase 1 versus the RFxPro slice.
