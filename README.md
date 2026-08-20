# RFx Platform

RFx Platform is the shared engineering platform for RF measurement collection, durable offline synchronization, ingestion, geospatial storage, visualization, and later operator analytics.

## Product family

- **RFxPro** — professional field-engineering Android product and first delivery target.
- **RFx Lite** — later lightweight/crowdsourced Android product.
- **RFx SDK** — later embeddable collection capability for operator applications.
- **RFx SmartCare / RTP** — later enterprise and real-time monitoring capabilities.
- **RFx Dashboard** — React and Leaflet web experience.

Repository name: `rfx-platform`. Product/platform name: **RFx Platform**.

## Current status: documentation baseline only

No application source code may be generated until this baseline has been reviewed and explicitly accepted. The files under `docs/` and the ADRs are the engineering source of truth. Source HLD/LLD material is supporting input; conflicts and version drift must be captured as open issues rather than silently resolved.

## Agreed technology direction

| Area | Direction |
|---|---|
| Android | Kotlin; shared core for RFxPro, RFx Lite, and future SDK; Room; WorkManager; offline-first |
| Backend | ASP.NET Core is the accepted main backend system; .NET Worker Services handle asynchronous/background processing (ADR-001) |
| Human identity | Keycloak with OIDC/OAuth 2.0 |
| Device identity | Owned by the RFx backend |
| Data | PostgreSQL 16 with PostGIS |
| Async work | Redis-backed queue |
| Objects | MinIO |
| Web maps | React, Leaflet, GeoServer, MapProxy, and reused OSM data/tiles subject to policy |
| Local deployment | Docker Compose |
| Delivery | Open/Developer capabilities first; Diag deferred; RFxPro vertical slice first |

## Documentation map

- [Project charter](docs/00-product/project-charter.md)
- [Scope](docs/01-requirements/scope.md)
- [RFxPro vertical-slice acceptance](docs/01-requirements/vertical-slice-acceptance.md)
- [System overview](docs/02-architecture/system-overview.md)
- [Engineering agreement](docs/08-standards/engineering-agreement.md)
- [Implementation roadmap](docs/07-roadmap/implementation-roadmap.md)
- [LLD reconciliation](docs/09-planning/lld-reconciliation.md)
- [Planning decision register](docs/09-planning/decision-register.md)
- [Baseline acceptance checklist](docs/09-planning/baseline-acceptance-checklist.md)
- [Architecture decisions](docs/11-adr/)

## Governance

Implementation is not complete until all affected documentation—including ADRs, API contracts, schemas, architecture, testing, and roadmap/status—is updated in the same change. See `AGENTS.md` and the engineering agreement.
