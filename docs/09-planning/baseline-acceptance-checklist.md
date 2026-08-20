# Documentation Baseline Acceptance Checklist

Status: **Draft — baseline not yet accepted**

## Purpose

This is the review gate that determines whether the repository contains enough approved information to initialize and implement the RFxPro vertical slice. Checking an item means the named reviewer verified the linked source-of-truth document; it does not mean a source HLD/LLD was accepted wholesale.

## A. Governance and authority

- [ ] Name the baseline approver(s) and reviewers.
- [ ] Record the exact commit proposed for acceptance.
- [ ] Confirm RFxPro LLD v1.6 reconciliation is complete for first-delivery topics.
- [ ] Confirm HLD v2.1 future work does not implicitly expand first-delivery scope.
- [ ] Record accepted exceptions, deferred decisions, owners, and blocking milestones.
- [ ] Complete the engineering-agreement acceptance record.

## B. Product and scope

- [ ] Approve the project charter and measurable first-slice outcome.
- [ ] Approve first delivery, subsequent RFxPro increments, and deferred products/capabilities.
- [ ] Approve the vertical-slice acceptance scenarios and evidence format.
- [ ] Assign product, RF/domain, Android, backend/data, security/privacy, and platform owner roles to named people.

## C. Architecture and decisions

- [x] Accept ASP.NET Core and .NET Worker Services as the main backend platform (ADR-001).
- [ ] Accept, revise, or explicitly defer ADR-002 through ADR-006.
- [ ] Approve Android shared-core boundaries for the first slice.
- [ ] Approve durable admission and API-to-worker handoff architecture.
- [ ] Approve initial deployment boundaries without treating Docker Compose as the production decision.

## D. Contracts and data

- [ ] Resolve DR-003 through DR-005.
- [ ] Produce and review the initial OpenAPI contract and examples.
- [ ] Produce and review the canonical measurement dictionary.
- [ ] Approve the logical first-slice data model, tenancy keys, idempotency constraints, and processing states.
- [ ] Confirm source LLD DDL is not treated as approved migration content.

## E. Identity, security, and privacy

- [ ] Resolve DR-006, DR-007, and DR-014.
- [ ] Document device bootstrap, storage, rotation, revocation, and recovery.
- [ ] Approve the initial tenant/role/permission matrix.
- [ ] Complete a first-slice threat model and abuse cases.
- [ ] Approve sensitive-data logging/redaction, retention, deletion, export, and test-data rules.

## F. Engineering and operations

- [ ] Resolve the project-initialization portions of DR-008 through DR-013.
- [ ] Pin required SDK, build-tool, and container versions.
- [ ] Define CI gates and evidence retention.
- [ ] Define health/readiness, logs, metrics, traces, and minimum alerting for the vertical slice.
- [ ] Define configuration and secret-management rules for local/integration environments.
- [ ] Define the minimum backup/restore expectations for pilot data before pilot rollout.

## G. Test and delivery plan

- [ ] Approve the supported real-device matrix and RF validation method.
- [ ] Map VS-01 through VS-12 to automated or named manual tests.
- [ ] Assign roadmap phases, milestones, dependencies, and entry/exit owners.
- [ ] Define pilot entry/exit criteria and release authority.
- [ ] Record risks whose acceptance is required for the first slice.

## Acceptance record

- Proposed baseline commit: TBD — owner: project owner
- Accepted by: TBD — owner: project owner
- Reviewed by: TBD — owners: component/security/privacy reviewers
- Accepted on: TBD
- Conditions/exceptions: TBD
- Decision-register snapshot: TBD

Until this record is completed, documentation-only work is permitted but application source, executable scaffolding, migrations, and deployable infrastructure remain prohibited.

