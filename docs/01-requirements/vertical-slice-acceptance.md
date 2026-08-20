# RFxPro Vertical-Slice Acceptance

Status: **Draft — awaiting baseline acceptance**

## Objective

Prove the smallest end-to-end RFxPro path from a supported real Android device to an authorized, traceable measurement stored in PostgreSQL/PostGIS, including ordinary offline operation and safe retry.

This document defines behavioral acceptance. Exact schemas, paths, limits, tool versions, and quantitative SLOs remain governed by the decision register and future OpenAPI/data artifacts.

## In scope

1. Register or provision one RFx-managed device identity using the approved credential lifecycle.
2. Create a client-stable RFxPro session identifier under the approved authoritative-ID contract.
3. Capture the approved minimum GPS and Open/Developer RF measurement set on a supported real Android device.
4. Persist each observation durably in Room before it becomes eligible for transmission.
5. Continue capture while network access is unavailable and through an approved restart/process-recovery scenario.
6. Form a versioned batch with a stable `batchId` and retain the pending data until durable server acknowledgement.
7. Authenticate and authorize the device, session, tenant/project scope, payload version, and batch.
8. Record durable admission/idempotency state before transient queue delivery is relied upon.
9. Process the admitted batch through a .NET Worker and commit normalized measurements to PostgreSQL/PostGIS idempotently.
10. Expose a traceable batch outcome through logs/telemetry and the approved status contract.

## Required acceptance scenarios

| ID | Scenario | Pass condition | Required evidence |
|---|---|---|---|
| VS-01 | Valid online session | Approved minimum RF/GPS observation reaches PostGIS with correct session/device/tenant association | Device/build record, request/batch correlation, database verification |
| VS-02 | Offline continuation | Capture persists locally during loss of connectivity without losing already committed observations | Room state and timestamped device evidence before/after reconnect |
| VS-03 | Safe reconnect | Pending data uploads after connectivity returns and receives the approved durable acknowledgement | Android sync evidence and correlated server status |
| VS-04 | Duplicate retry | Repeating the same accepted `batchId` creates no duplicate domain measurements and returns the approved duplicate semantic result | API response/status and database row comparison |
| VS-05 | Ambiguous response | If the server admits a batch but the client does not receive the response, retry converges without loss or duplication | Fault-injection evidence across client, API, worker, and database |
| VS-06 | Restart recovery | Approved process/app restart scenario preserves or resumes pending work safely | Android instrumentation/manual device evidence |
| VS-07 | Invalid payload | Invalid schema/version/measurement content is rejected with a stable, non-sensitive error and does not enter normal processing | Contract test, error response, and database/queue verification |
| VS-08 | Worker failure | Retryable work follows bounded retry policy; permanent failure becomes visible in a queryable dead-letter state | Worker logs/metrics/status evidence |
| VS-09 | Authorization isolation | A device cannot write to an unauthorized tenant, project, session, or another device's scope | Security integration test evidence |
| VS-10 | Sensitive-data handling | Credentials/full tokens are absent from Room, source, logs, error bodies, and committed examples | Secure-storage verification and log/repository scan |
| VS-11 | Geospatial correctness | Stored coordinates use the approved SRID/order and can be queried as the expected point | Database query and known-location comparison |
| VS-12 | Traceability | One correlation path connects device/session/batch through API admission, worker outcome, and stored rows | Correlation identifiers and evidence record |

## Minimum evidence record

Each acceptance run records:

- date/time and reviewer;
- device model, Android version, permissions, access capability, and app build;
- operator/network and privacy-safe location/test context;
- API and worker build identifiers;
- schema and contract version;
- session ID, batch ID, and non-secret correlation identifiers;
- scenario results and links to automated/manual evidence;
- deviations, known limitations, and follow-up owner.

## Explicit exclusions

The first vertical slice does not require speed tests, cell-reference matching, maps/dashboard, exports, Keycloak browser login, RFx Lite, SDK embedding, advanced analytics, SmartCare/RTP, call/OTT testing, or Diag/L3 capabilities. Their absence does not fail this acceptance.

## Unresolved acceptance inputs

The following must be approved through the planning decision register before this specification can be baselined:

- DR-003 through DR-007: identity, contracts, measurement definition, and authorization;
- DR-009 and DR-010: durable handoff and Android synchronization policy;
- DR-011: supported pilot device matrix;
- DR-012 through DR-014: toolchain, data access, and privacy requirements;
- the baseline-critical limits from DR-015 and observability requirements from DR-016.

