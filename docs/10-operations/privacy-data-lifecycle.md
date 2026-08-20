# Privacy and Data Lifecycle Plan

Status: **Draft — privacy/legal and product approval required**

## Classification

Treat precise location, timestamped RF measurements, session/project association, device metadata, network/operator identifiers, and exports as **sensitive operational data** until an authorized privacy review assigns a stricter or more specific classification. Credentials and secret material are **restricted secrets** and must never be stored with measurement data.

## Purpose and minimization

The first slice collects only fields needed to validate RFxPro engineering capture, offline synchronization, ingestion correctness, and geospatial storage. Lite/SDK crowdsourcing, advertising, subscriber profiling, regulator reporting, data sales, SmartCare/RTP, and Diag/L3 processing are not covered by this first-slice purpose.

Do not collect optional device identifiers, phone numbers, subscriber identities, IMSI/IMEI, contact data, application usage, or arbitrary payload extensions unless a later approved requirement and privacy review explicitly authorizes them.

## Consent and transparency

- RFxPro foreground collection must be explicit to the engineer and comply with Android permission/notification requirements.
- The pilot owner must document the lawful/contractual basis and operator/customer authorization for location and RF collection.
- RFx Lite and SDK consent flows require separate future reviews and are not inherited from RFxPro approval.
- Test target phone numbers, if call testing is later approved, require separate classification and handling rules.

## Lifecycle stages

| Stage | Required rule |
|---|---|
| Capture | Persist locally before upload; show required foreground/permission transparency |
| Local pending | Protect with application/OS controls; credentials remain outside Room |
| Transmission | TLS; authenticate and authorize; no secret/payload logging |
| Admission/processing | Tenant scope, idempotency, minimum necessary access, audited administrative actions |
| Storage/query | Tenant/project authorization; raw and derived data separated |
| Export | Explicit permission, audit, expiry/lifecycle metadata, no public buckets |
| Backup | Same or stronger classification/access as source data; restore access audited |
| Retention/deletion | Approved schedule and legal holds; deletion must cover replicas/objects according to policy |

## Retention decision template

The project owner and privacy/legal reviewer must approve values rather than inheriting the source HLD proposal:

- pending Android data: TBD duration/size policy;
- acknowledged Android session history: TBD;
- raw admitted batch payload: TBD;
- normalized measurements: TBD;
- derived aggregates: future scope, TBD;
- audit/security events: TBD;
- rejected/dead-letter payload and diagnostic metadata: TBD;
- exports and temporary download authorization: TBD;
- backups and deletion propagation: TBD.

For each category record purpose, owner, tenant/customer obligation, retention trigger, deletion/anonymization method, legal-hold behavior, backup propagation, and verification evidence.

## Data subject and customer requests

Before collecting pilot data, define who can authorize access, correction where applicable, project/session deletion, tenant offboarding, export, and incident disclosure. Engineering must not promise deletion behavior that backups or immutable audit obligations cannot meet.

## Test data

- Prefer synthetic fixtures in unit, integration, CI, demos, and documentation.
- Real pilot data must not be copied into developer laptops, CI artifacts, screenshots, issue trackers, or sample payloads without explicit approval and redaction.
- Record provenance and approval for any reference dataset used to validate RF accuracy.

