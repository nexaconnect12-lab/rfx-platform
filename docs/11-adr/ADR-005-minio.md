# ADR-005: MinIO Object Storage

- Status: Proposed
- Date: 2026-08-19

## Context

Exports, imports, reports, and other large artifacts should not be stored as large relational values or local service files.

## Decision

Use MinIO as S3-compatible object storage. Store object ownership, tenant scope, checksum, media type, lifecycle/status, and authorization metadata in PostgreSQL.

## Consequences

Access uses short-lived authorized flows; buckets are not public by default. Retention, encryption, antivirus/content validation, multipart limits, backup, and production S3 compatibility remain open.

