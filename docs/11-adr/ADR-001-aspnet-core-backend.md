# ADR-001: ASP.NET Core Backend

- Status: Proposed
- Date: 2026-08-19

## Context

Source designs allowed Node.js or Kotlin/Ktor and emphasized responsibilities rather than a mandatory runtime. The project subsequently selected .NET.

## Decision

Use ASP.NET Core for HTTP APIs and .NET Worker Services for background processing. Begin with modular boundaries and a limited number of deployables; do not create a microservice per domain by default.

## Consequences

.NET provides one backend ecosystem for APIs/workers and strong PostgreSQL, JWT/OIDC, observability, and test support. Exact .NET version, persistence library, queue framework, and module boundaries remain to be pinned. This decision supersedes earlier source suggestions of Node.js/Fastify, BullMQ, or Ktor.

