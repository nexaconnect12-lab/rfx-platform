# System Overview

Status: **Draft — awaiting baseline acceptance**

## Context

```text
RFxPro Android (first) ─┐
RFx Lite (later) ───────┼─ device credentials ─> ASP.NET Core API
RFx SDK (later) ────────┤                         │
SmartCare/RTP (later) ──┘                         ├─> Redis queue ─> .NET Worker
                                                  │                    │
React + TypeScript + Ant Design                    │                    │
React Leaflet/Leaflet ─ OIDC/PKCE ─ Keycloak ─ JWT┤                    ├─> PostgreSQL/PostGIS
                                                  │                    └─> MinIO
                                                  └─ query APIs

PostGIS ─> GeoServer ─> WMS/WFS/vector services ─> Leaflet
OSM source/cache ─> MapProxy ─────────────────────> Leaflet
```

## Boundaries

- Android shared core owns supported collection, local persistence, synchronization, and shared domain behavior.
- ASP.NET Core owns public application APIs, validation, authorization, idempotency admission, and queries.
- .NET Worker Services own queued ingestion, transformations, derived data, exports, and scheduled work.
- Keycloak owns human authentication and token issuance, not RFx domain authorization.
- RFx owns device identity and business/tenant authorization.
- Ant Design owns the dashboard UI design system; React Leaflet/Leaflet owns map rendering and geospatial interaction.
- PostgreSQL/PostGIS is the system of record for operational and geospatial domain data.
- Redis is transient coordination/queue infrastructure, not the system of record.
- MinIO stores large immutable/export objects; metadata and authorization remain in PostgreSQL.

## Deployment direction

Docker Compose is the initial local and integration topology. Under ADR-009, the backend begins as one principal ASP.NET Core modular-monolith deployment plus one or a small number of separately deployable .NET workers. Domain modules are not microservices by default. Future extraction requires operational evidence and a new ADR.

## Open issues

- Select production queue library/protocol and delivery semantics.
- Define GeoServer/MapProxy deployment ownership, caching, and data-publication lifecycle.
- Decide boundaries for speed-test service, analytics, notifications, and exports.
- Establish production hosting, backup, disaster recovery, monitoring, and secrets platforms.
