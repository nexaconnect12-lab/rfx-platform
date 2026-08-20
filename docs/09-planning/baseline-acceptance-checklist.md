# Documentation Baseline Acceptance Checklist

Status: **Draft — baseline not yet accepted**

## Purpose

This is the review gate that determines whether the repository contains enough approved information to initialize and implement the RFxPro vertical slice. Checking an item means the named reviewer verified the linked source-of-truth document; it does not mean a source HLD/LLD was accepted wholesale.

## A. Governance and authority

- [ ] Name the baseline approver(s) and reviewers.
- [ ] Record the exact commit proposed for acceptance.
- [x] RFxPro LLD v1.6 reconciliation document exists for first-delivery topics; human confirmation remains required.
- [x] HLD v2.1 future work is explicitly prevented from implicitly expanding first-delivery scope; human confirmation remains required.
- [ ] Record accepted exceptions, deferred decisions, owners, and blocking milestones.
- [ ] Complete the engineering-agreement acceptance record.

## B. Product and scope

- [ ] Approve the project charter and measurable first-slice outcome.
- [ ] Approve first delivery, subsequent RFxPro increments, and deferred products/capabilities.
- [ ] Approve the vertical-slice acceptance scenarios and evidence format.
- [ ] Assign product, RF/domain, Android, backend/data, security/privacy, and platform owner roles to named people.

## C. Architecture and decisions

- [x] Accept ASP.NET Core and .NET Worker Services as the main backend platform (ADR-001).
- [x] Accept pragmatic Domain-Driven Design for backend business rules and module boundaries (ADR-008); detailed aggregate and solution structure remain initialization decisions.
- [x] Accept a modular monolith plus separately deployable .NET workers as the initial backend architecture; microservices require evidence and a new ADR (ADR-009).
- [x] Accept React + TypeScript + Ant Design as the dashboard design system and React Leaflet/Leaflet for maps (ADR-007); implementation remains deferred to the dashboard phase.
- [ ] Accept, revise, or explicitly defer ADR-002 through ADR-006.
- [ ] Approve Android shared-core boundaries for the first slice.
- [ ] Approve durable admission and API-to-worker handoff architecture.
- [ ] Approve initial deployment boundaries without treating Docker Compose as the production decision.

## D. Contracts and data

- [ ] Resolve DR-003 through DR-005.
- [x] Produce the initial OpenAPI contract; human review/acceptance remains required.
- [x] Produce the canonical measurement dictionary; RF/domain review/acceptance remains required.
- [ ] Approve the logical first-slice data model, tenancy keys, idempotency constraints, and processing states.
- [ ] Confirm source LLD DDL is not treated as approved migration content.

## E. Identity, security, and privacy

- [ ] Resolve DR-006, DR-007, and DR-014.
- [x] Document the recommended device bootstrap, storage, rotation, revocation, and recovery model; policy values and approval remain required.
- [ ] Approve the initial tenant/role/permission matrix.
- [x] Produce a first-slice threat model and abuse cases; security/privacy review remains required.
- [ ] Approve sensitive-data logging/redaction, retention, deletion, export, and test-data rules.

## F. Engineering and operations

- [ ] Resolve the project-initialization portions of DR-008 through DR-013.
- [ ] Pin required SDK, build-tool, and container versions.
- [x] Define proposed CI/test gates and evidence categories; thresholds and retention approval remain required.
- [x] Define proposed health/readiness, logs, metrics, traces, and minimum signals; platform and alert thresholds remain required.
- [ ] Define configuration and secret-management rules for local/integration environments.
- [ ] Define the minimum backup/restore expectations for pilot data before pilot rollout.

## G. Test and delivery plan

- [ ] Approve the supported real-device matrix and RF validation method.
- [x] Map VS-01 through VS-12 to automation targets and manual evidence; named assignments remain required.
- [x] Define roadmap milestone gates, dependencies, and owner roles; names/dates/estimates remain required.
- [x] Define proposed pilot entry/exit criteria; release authority remains TBD.
- [ ] Record risks whose acceptance is required for the first slice.

## Acceptance record

- Proposed baseline commit: TBD — owner: project owner
- Accepted by: TBD — owner: project owner
- Reviewed by: TBD — owners: component/security/privacy reviewers
- Accepted on: TBD
- Conditions/exceptions: TBD
- Decision-register snapshot: TBD

Until this record is completed, documentation-only work is permitted but application source, executable scaffolding, migrations, and deployable infrastructure remain prohibited.
