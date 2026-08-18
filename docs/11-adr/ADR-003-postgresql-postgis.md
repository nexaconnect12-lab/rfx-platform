# ADR-003: PostgreSQL 16 and PostGIS

- Status: Proposed
- Date: 2026-08-19

## Context

RFx data is relational, time-oriented, geospatial, and queried by session, operator/project, location, and time.

## Decision

Use PostgreSQL 16 with PostGIS as the RFx system of record. Manage changes through migrations and use database constraints for integrity, tenant keys, and idempotency where practical.

## Consequences

The team must define SRID, measurement schema, indexing/partitioning, retention, backup/restore, and privacy deletion. Source DDL variants are not accepted automatically; drift is an open issue until reconciled.

