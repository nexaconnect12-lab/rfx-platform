# Operational Readiness Plan

Status: **Draft — quantitative targets require owner approval**

## Environments

| Environment | Purpose | Data policy | Minimum controls |
|---|---|---|---|
| Local | Developer workflow after baseline acceptance | Synthetic/non-sensitive only | Reproducible Compose, local secrets outside Git, health checks |
| Integration/CI | Automated contract, integration, migration, and failure tests | Synthetic fixtures only | Ephemeral/isolation policy, deterministic initialization, evidence retention |
| Pilot | Approved real-device/operator validation | Approved pilot data only | TLS, access control, monitoring, backups, incident owner, retention policy |
| Production | Future customer service | Requires separate readiness approval | SLOs, HA/capacity, DR, security/privacy approval, runbooks, support model |

Docker Compose is the initial local/integration topology. It is not automatically the production deployment decision.

## Observability baseline

All API and worker activity uses structured logs and OpenTelemetry-compatible traces/metrics where supported. Required correlation dimensions are tenant-safe identifiers for request, device, session, batch, outbox message, worker attempt, and deployment version.

Never record credentials, authorization headers, complete request payloads, precise coordinates, full device identifiers unnecessary for diagnosis, or cross-tenant details in ordinary logs.

Minimum signals:

- API request count, latency, response class, validation rejection, authorization denial, throttling, and admission failures;
- admitted, duplicate, processing, processed, rejected, dead-letter, and oldest-pending batch counts/age;
- outbox backlog/publication failures and worker retry duration;
- PostgreSQL/Redis/MinIO connectivity and capacity indicators;
- Android sync attempt/outcome categories without secrets or raw payloads;
- audit events for device lifecycle, access, export, and administrative changes.

## Health semantics

- **Liveness:** the process event loop/runtime is responsive; it must not fail solely because a dependency is temporarily unavailable.
- **Readiness:** the service can perform its required role, including critical dependency access.
- **Startup:** initialization/migrations/configuration checks have completed where a separate startup probe is available.

Detailed endpoint names, exposure, authentication, and dependency thresholds remain deployment decisions.

## Incident and recovery runbooks required before pilot

- revoke/replace a compromised device credential;
- diagnose a stuck admitted batch and replay/reconcile safely;
- inspect and resolve dead-letter work without bypassing tenant authorization;
- restore PostgreSQL and verify application consistency;
- restore/reconcile MinIO objects and PostgreSQL metadata where used;
- rotate service secrets and certificates;
- respond to sensitive-data exposure in logs or exports;
- suspend ingestion without losing locally retained Android data;
- roll back application deployment while preserving migration compatibility.

## Backup and recovery

Before pilot approval, owners must specify and test:

- PostgreSQL backup method, schedule, encryption, retention, restore owner, RPO, and RTO;
- MinIO replication/backup and metadata consistency where object storage is used;
- Keycloak backup when human identity enters scope;
- secret/configuration recovery without storing secrets in documentation;
- restore rehearsal evidence and acceptance authority.

No numeric RPO/RTO or availability promise is approved by this draft.

## Pilot readiness gate

Pilot entry requires TLS, approved identity/tenancy, monitoring ownership, alert routing, privacy/retention approval, tested credential revocation, tested database restore, support contact, rollback plan, capacity check, and completion of the vertical-slice acceptance scenarios assigned to the pilot milestone.

