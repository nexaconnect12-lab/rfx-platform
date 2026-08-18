# Testing Strategy

Status: **Draft — awaiting baseline acceptance**

## Quality model

Testing follows risk: measurement correctness, offline durability, idempotency, security/tenancy, migration safety, and device variability have the highest priority.

## Layers

- Unit tests for domain rules, normalization, state machines, authorization, batching, and retry decisions.
- Property/contract tests for measurement validation and idempotency invariants.
- Android instrumentation tests for Room migrations, persistence, WorkManager, process death, permissions, and lifecycle.
- Backend integration tests using real PostgreSQL/PostGIS, Redis, MinIO, and representative Keycloak tokens/configuration.
- OpenAPI consumer/compatibility tests across Android, API, and dashboard.
- End-to-end tests from device/captured fixture through worker and PostGIS query.
- Security tests for tenant isolation, token validation, revocation, privilege escalation, and sensitive logging.
- Performance/soak tests for long sessions, one-second sampling, backlog recovery, large imports, and map queries.
- Backup/restore and migration rehearsal before production releases.

## Initial release gates

- Compare RSRP/RSRQ/SINR and identifiers against an approved reference on supported real devices.
- Collect with screen locked/background conditions permitted by Android policy.
- Continue offline, restart safely, reconnect, and upload without loss.
- Resubmit identical `batchId` and prove no duplicate domain rows.
- Process a representative two-hour, one-second session within approved SLOs.
- Route malformed/permanently failing work to observable dead-letter handling.
- Prove cross-tenant access is denied.

## Evidence

Each implementation records automated results and any manual/device evidence. Device, OS, app build, permissions, operator/network, time, and reference tool/version are captured for RF validation.

## Open issues

- Approve reference equipment/tolerances, supported-device matrix, SLOs, data volumes, and CI environment.
- Define privacy-safe test datasets and synthetic fixtures.
- Decide release-blocking coverage thresholds and ownership of manual field tests.

