# Data Architecture

Status: **Draft — awaiting baseline acceptance**

## Direction

PostgreSQL 16 with PostGIS is the authoritative operational/geospatial store. Redis is transient queue/cache coordination. MinIO stores large objects and exports, while PostgreSQL stores ownership, metadata, checksums, lifecycle, and authorization context.

Keycloak uses its own database or isolated Keycloak-owned schema and migrations. It must not modify RFx domain tables.

## Candidate domain model

- organizations/tenants, operators, projects;
- RFx user profiles mapped to Keycloak subject IDs;
- devices and credential lifecycle metadata;
- sessions and explicit processing state history;
- ingestion batches with unique device/session/batch identity and processing/audit fields;
- measurements with location, RF values, source/capability/quality metadata, and capture time;
- speed tests and experimental call tests;
- cell-reference datasets, versions, imports, and records;
- exports/object metadata;
- audit events, worker/dead-letter records, and schema/reference versions.

This is a conceptual list, not approved DDL.

## Data rules

- Use migrations; never mutate production schemas manually.
- Document units, ranges, nullability, coordinate system/SRID, timezone, and provenance for every measurement.
- Enforce tenant scope and idempotency with database constraints where possible.
- Keep raw facts separate from derived/aggregated values.
- Define retention, deletion, anonymization, backup, restore, and partitioning before production scale.
- Spatial and time indexes must be driven by verified query patterns.

## Open issues

- Reconcile source DDL versions and missing operational tables; do not select one silently.
- Approve tenancy keys, authoritative session ID, ingestion uniqueness constraint, and measurement primary-key strategy.
- Approve SRID and representation for GPS uncertainty, altitude, and invalid fixes.
- Establish privacy classification and location-data retention/deletion policy.
- Define partitioning, archival, backup/restore objectives, and MinIO lifecycle.

