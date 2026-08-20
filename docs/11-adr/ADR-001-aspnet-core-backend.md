# ADR-001: ASP.NET Core Backend

- Status: Accepted
- Decision date: 2026-08-20
- Accepted by: Project owner

## Context

Source designs, including RFxPro LLD v1.6, allowed Node.js or Kotlin/Ktor and emphasized responsibilities rather than a mandatory runtime. The project subsequently selected .NET as the main backend platform.

## Decision

Use ASP.NET Core as the main backend system for HTTP APIs and .NET Worker Services for asynchronous and background processing. Begin with modular boundaries and a limited number of deployables; do not create a microservice per domain by default.

Node.js, Express, Fastify, Kotlin/Ktor, and BullMQ examples in source HLD/LLD material are historical inputs and are not implementation alternatives. A future change of backend platform requires a superseding ADR.

## Consequences

.NET provides one backend ecosystem for APIs/workers and strong PostgreSQL, JWT/OIDC, observability, and test support. Exact .NET version, persistence library, Redis-compatible queue framework, and module boundaries remain to be pinned. This decision supersedes earlier source suggestions of Node.js/Fastify, BullMQ, or Ktor without accepting the source documents' detailed contracts, schemas, or deployment examples.
