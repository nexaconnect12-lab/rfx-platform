# Engineering Agreement

Status: **Draft — awaiting acceptance**

## 1. Authority and change control

The accepted repository documentation, contracts, schemas, and ADRs are the engineering source of truth. Source HLD/LLD files remain evidence and intent inputs. Conflicts, ambiguities, and version drift are recorded as open issues with provenance; they are never silently reconciled.

Accepted ADRs are immutable decision records. A changed decision requires a new superseding ADR and migration/compatibility plan.

## 2. Pre-implementation gate

No agent or contributor may generate application source code, executable service scaffolding, migrations, or deployable infrastructure until the owner records explicit acceptance of this baseline. Review edits to documentation are permitted.

## 3. Definition of done — mandatory documentation rule

After **every implementation**, the contributor or agent must update every affected source-of-truth artifact before work is considered complete:

- requirements/scope and acceptance criteria;
- architecture diagrams, boundaries, and data flows;
- ADRs for decisions or supersessions;
- API/OpenAPI contracts, examples, and compatibility notes;
- data model, schema documentation, migrations, retention, and backfill notes;
- authentication, authorization, privacy, threat, and operational guidance;
- test strategy/cases and verification evidence;
- environment/runbooks and roadmap/status/open issues.

Documentation and implementation ship in the same change. If no document changes are required, the completion record names the documents reviewed and why they remain correct. Work cannot be described as done while documentation knowingly disagrees with behavior.

## 4. Technical agreements

- Kotlin Android with an offline-first Room/WorkManager shared core; RFxPro first.
- ASP.NET Core as the main backend system plus .NET Worker Services, as accepted in ADR-001; source Node.js/Ktor/BullMQ examples are superseded.
- Keycloak for human OIDC/OAuth2 identity; RFx backend for device identity and business authorization.
- PostgreSQL 16/PostGIS as system of record; Redis-backed queue; MinIO object storage.
- React/Leaflet with GeoServer, MapProxy, and policy-compliant OSM reuse.
- Docker Compose for reproducible local/integration environments.
- Open/Developer capabilities first; Diag deferred.
- Contract-first, idempotent ingestion and backward-compatible evolution.

## 5. Quality and safety

Changes require proportionate tests, review, secure secret handling, tenant isolation, observability, reversible migrations, and recovery consideration. Location/RF data is treated as potentially sensitive. Real-device evidence is required where Android/modem behavior cannot be simulated reliably.

## 6. Open-issue protocol

An open issue states: the conflicting/unknown facts, source/version where known, impact, decision owner, and blocking milestone. An assumption may enable exploration only when clearly labeled and must not silently become a permanent contract.

## Acceptance record

- Acceptance checklist: `docs/09-planning/baseline-acceptance-checklist.md`
- Baseline version/commit: TBD
- Accepted by: TBD
- Accepted on: TBD
- Conditions/exceptions: TBD
