# First-Slice Threat Model

Status: **Draft — security/privacy review required**

## Protected assets

- device and bootstrap credentials;
- tenant/project authorization data;
- precise location, timestamps, RF measurements, and device metadata;
- ingestion integrity and idempotency state;
- operational logs, audit records, backups, and object storage;
- Keycloak configuration and human identities when introduced.

## Trust boundaries

1. Android device and OS secure storage.
2. Public/untrusted network to the RFx API over TLS.
3. API authorization and durable admission boundary.
4. PostgreSQL, Redis, MinIO, and worker service identities.
5. Human identity boundary between Keycloak and RFx domain authorization.
6. Administrative/support access and backup systems.

## Threats and required controls

| Threat | First-slice controls | Verification |
|---|---|---|
| Stolen/replayed device credential | High-entropy opaque token, TLS, server-side status/revocation, rotation, no token logging | Revocation and replay tests |
| Bootstrap abuse | Single-use scoped bootstrap, expiry/attempt limits TBD, audited issuance | Negative registration tests |
| Cross-tenant access | Immutable tenant scope, backend policy enforcement, constrained queries | Tenant-isolation integration tests |
| Forged session/project scope | Derive scope from authorized device/session; reject client scope escalation | Authorization tests |
| Duplicate or altered retry | Batch uniqueness plus immutable digest; conflict on same ID/different content | Idempotency contract tests |
| Accepted data lost between API and queue | Transactional durable admission and outbox; reconciliation | Fault injection before/after publish |
| Malformed/oversized ingestion | Schema/version validation, approved limits, rate limiting, bounded parsing | Fuzz/limit/contract tests |
| Sensitive logs/errors | Allowlisted structured fields, redaction, no payload/token logging | Automated scan and review |
| Redis/worker replay | At-least-once-safe consumer, inbox/idempotency, bounded retry, dead-letter state | Duplicate-delivery tests |
| Database/object theft | Least-privilege service identities, encryption/host controls, backup access policy TBD | Configuration and restore review |
| Compromised device fabricates measurements | Device status, provenance, capability matrix, anomaly/audit evidence; attestation deferred | Field comparison and anomaly review |
| Location privacy misuse | Purpose limitation, tenant authorization, retention/deletion approval, export audit | Privacy review and access tests |
| Supply-chain compromise | Pinned dependencies/images, lock files, vulnerability and provenance checks | CI evidence after initialization |

## Explicit non-claims

The first slice does not claim resistance to a fully compromised/rooted device, certified telecom measurement accuracy, anonymous national crowdsourcing, Diag/L3 legality, or production-grade disaster recovery until the corresponding reviews and controls are accepted.

## Required review decisions

- privacy classification and retention/deletion;
- bootstrap and device-credential policy;
- tenant and support-access model;
- secret-management platform;
- TLS/domain/certificate ownership;
- incident owner and notification path;
- backup encryption/access and pilot RPO/RTO;
- whether attestation, certificate-bound tokens, or mTLS are needed after pilot evidence.

