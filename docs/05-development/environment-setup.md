# Development Environment Setup

Status: **Draft — awaiting baseline acceptance**

## Baseline gate

This document records the intended environment only. Do not initialize application projects or generate source until the documentation baseline is accepted.

## Host tools

- Git.
- Docker Desktop with WSL2 integration on Windows (or a compatible Docker Engine host).
- A current stable IDE/editor for .NET and web development.
- Stable Android Studio with bundled JDK/JBR initially; exact versions are pinned when initialization is approved.
- Android SDK platform/build/platform/command-line tools, ADB, and emulator.
- At least one real supported Android phone; multiple OEM/API levels are strongly recommended.
- Optional database/API clients such as DBeaver and a repository-stored API client collection.

## Local services after acceptance

Docker Compose will provide PostgreSQL 16/PostGIS, Redis, MinIO, Keycloak, API, worker, and later GeoServer, MapProxy, and dashboard services. Developers should not depend on individually installed Windows server instances.

## Configuration rules

- Commit safe `.env.example`-style names/defaults only; never commit secrets.
- Pin SDK/tool/container versions in the repository.
- Use health checks and deterministic initialization.
- Keep developer setup reproducible across machines.
- Document migrations, seed data, ports, test accounts, and reset/recovery processes once chosen.

## Verification target

After project initialization is approved, the first environment check is service health for PostGIS, Redis, MinIO, and Keycloak, followed by a real-device end-to-end vertical-slice test.

## Open issues

- Pin .NET SDK, Android toolchain, Node/pnpm (for React), container image versions, and supported host OS versions.
- Decide whether the canonical repository location is Windows or the WSL filesystem; prior guidance preferred WSL while the selected folder is `D:\DevOps\rfx-platform`.
- Approve local domain/TLS, port allocation, secret management, and seed identity strategy.

