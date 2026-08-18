# Scope

Status: **Draft — awaiting baseline acceptance**

## First delivery: RFxPro vertical slice

In scope:

- Android session creation using a client-generated stable UUID.
- Real-device capture of available GPS and Open/Developer Android RF measurements.
- Durable local Room persistence before network transmission.
- WorkManager-driven, retryable, batched synchronization.
- RFx-managed device registration/authentication.
- ASP.NET Core ingestion endpoints and .NET background processing.
- Redis-backed queue, PostgreSQL 16/PostGIS storage, MinIO where object storage is needed.
- Idempotent ingestion and traceable processing states.
- Operational verification from device to stored geospatial data.

## Subsequent RFxPro increments

Session UI/history, maps, neighbor/cell reference matching, cell-file import, speed tests, exports, Keycloak-backed dashboard, GeoServer layers, MapProxy/OSM basemap reuse, reporting, and production hardening.

## Deferred

- Diag/privileged modem capabilities.
- RFx Lite continuous/crowdsourced collection.
- RFx SDK and third-party embedding/consent flows.
- RFx SmartCare/RTP and enterprise real-time monitoring.
- Advanced/busy-hour analytics and independent microservice decomposition.

## Cross-cutting requirements

- No measurement loss during ordinary offline/retry scenarios.
- Duplicate submissions must not create duplicate domain records.
- Tenant and project/operator authorization must be enforced in the backend.
- Secrets must use platform-secure storage; Android tokens must not be stored in Room.
- Collection must be transparent and permission/consent compliant.
- Every material workflow must be observable and testable.

## Not yet approved

Sampling defaults, data retention, throughput/latency SLOs, exact supported RF fields by Android API/device, speed-test methodology, call-test scope, export limits, role-to-permission matrix, tenancy model, and MVP dashboard boundary.

## Open issues

- Confirm whether client-generated session UUID is authoritative across client and server.
- Define batch size, retry/backoff, retention, and quota limits.
- Define measurement units, nullability, timestamps, coordinate reference system, and quality flags.
- Decide whether call tests are a separate experimental track or an RFxPro release requirement.

