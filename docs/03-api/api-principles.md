# API Principles

Status: **Draft — awaiting baseline acceptance**

## Contract first

OpenAPI is the canonical HTTP contract once implementation begins. API changes update OpenAPI, examples, consumers, tests, and this documentation in the same change. Do not infer a contract only from controller code.

The proposed first-slice contract is recorded in [First-Slice API Contract](first-slice-contract.md) and [OpenAPI](openapi.yaml). It is subject to baseline acceptance.

## Conventions

- HTTPS JSON APIs with explicit media type and API version strategy.
- UTC timestamps in ISO 8601; identifiers are opaque UUIDs unless specified otherwise.
- Consistent problem-details errors with stable machine-readable codes and correlation IDs.
- Pagination, filtering, sorting, maximum payload size, and rate limits are explicit per endpoint.
- Additive compatible evolution is preferred; breaking changes require a version/migration plan.
- Logs and error bodies must not expose secrets or unnecessary personal/location data.

## Offline ingestion

- The client creates the session UUID and `batchId`; the server contract must explicitly confirm whether that session UUID is authoritative.
- Every upload carries a schema version and stable idempotency key.
- Repeating an accepted batch returns the same semantic result and creates no duplicate measurements.
- `202 Accepted` means admitted for asynchronous processing, not completed.
- A status resource exposes admitted, processing, processed, rejected, and dead-letter outcomes.
- Session upload completion is distinct from worker processing completion.

## Initial resource candidates

Devices/registration, sessions, measurement batches, batch status, session upload completion/status, speed tests, cell-reference imports, exports, map/query data, and administrative resources. Exact paths and payloads remain unapproved.

## Open issues

- Approve the proposed `/v1` path/version convention, status semantics, and error model.
- Approve compression, payload/rate limits, and timestamp precision/skew.
- Approve the canonical [measurement dictionary](measurement-dictionary.md), including RAT-specific normalization and quality flags.
- Define idempotency record retention and behavior after retention expiry.
- Approve the recommended durable acknowledgement and safe local cleanup contract.
