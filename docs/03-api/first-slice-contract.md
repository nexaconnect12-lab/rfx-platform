# First-Slice API Contract

Status: **Proposed — awaiting baseline acceptance**

The companion [OpenAPI document](openapi.yaml) is the proposed canonical HTTP contract for the RFxPro vertical slice. This document records the intended semantics that cannot safely be inferred from paths or schemas alone.

## Recommended contract decisions

### Identifiers

- The Android client generates UUIDv4 `sessionId` and `batchId` values before transmission.
- The client-generated `sessionId` is authoritative end to end. The server does not substitute another public session identifier.
- A batch is uniquely identified by `(tenantId, deviceId, sessionId, batchId)`.
- Reuse of a `batchId` with a different payload digest is a conflict and must not overwrite the admitted batch.

These recommendations resolve ambiguity for offline creation and retries but remain subject to baseline acceptance under DR-003 and DR-004.

### Admission and acknowledgement

- A successful batch submission returns `202 Accepted` only after the API has committed the batch envelope, payload reference/content, digest, authorization scope, and idempotency record to durable storage in one transaction.
- `202 Accepted` means admitted for processing; it does not mean measurement processing is complete.
- Retrying an identical admitted batch returns the same batch resource identity and semantic state without adding measurements.
- Android may mark a batch durably acknowledged after `202 Accepted` or an equivalent duplicate response, but it must retain local session history according to the approved device-retention policy.
- Android must not delete or mark a batch acknowledged on timeout, connection loss, `5xx`, or an unrecognized response.

### Processing status

Proposed states:

- `ADMITTED`: durable admission completed.
- `PROCESSING`: a worker owns an active attempt.
- `PROCESSED`: normalized domain rows committed.
- `REJECTED`: permanent validation failure discovered after admission.
- `DEAD_LETTER`: retry policy exhausted or manual intervention required.

State transitions are append-audited. A status response includes correlation ID, attempt count, timestamps, and a safe error code where applicable.

### Session completion

Session upload completion means the client declares that it has no more batches for the session at that time. It does not imply every admitted batch is processed. A session becomes fully processed only when all admitted batches reach a terminal successful state under the approved state machine.

## Error behavior

- Use RFC 9457-style problem details with stable RFx error codes.
- Do not expose credentials, token claims not needed by the client, SQL, stack traces, internal hostnames, raw location payloads, or cross-tenant resource existence.
- Return the same not-found semantic result for absent and unauthorized cross-tenant resources where disclosure would create an enumeration risk.
- Every response includes or echoes a non-secret correlation identifier.

## Limits requiring approval

The OpenAPI contract names limits but does not invent their values. Before baseline acceptance, owners must approve:

- maximum observations per batch;
- maximum compressed and uncompressed request sizes;
- timestamp skew and session-duration rules;
- submission rate limits and burst policy;
- idempotency record retention;
- polling interval guidance and status retention;
- supported content encoding and compression.

## Compatibility

- The first version uses `/v1` paths and a required payload `schemaVersion`.
- Additive optional fields are preferred.
- A new required field, changed unit/meaning, removed enum value, or incompatible validation rule requires a compatibility plan and normally a new schema/API version.
- OpenAPI, Android models, examples, contract tests, measurement dictionary, and migration documentation change together.

