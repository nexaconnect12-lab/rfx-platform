# Toolchain Selection Record

Status: **Draft — versions must be pinned at baseline acceptance or project initialization approval**

## Accepted ecosystems

- Backend API and workers: ASP.NET Core and .NET Worker Services under ADR-001.
- Android: Kotlin with Room and WorkManager; ADR-006 remains proposed.
- Data: PostgreSQL 16/PostGIS; ADR-003 remains proposed.
- Human identity: Keycloak; ADR-002 remains proposed.
- Queue coordination: Redis-backed; ADR-004 remains proposed.
- Object storage: MinIO where required; ADR-005 remains proposed.
- Dashboard later: React/Leaflet with a pinned Node.js package manager/runtime.

## Version-selection policy

Use a currently supported stable/LTS release that is compatible with all selected dependencies and the team's deployment environment. Do not choose preview, nightly, end-of-support, or floating container tags. Record exact versions and image digests in repository-controlled configuration only after the documentation baseline authorizes initialization.

## Pinning record

| Component | Selected version/digest | Decision owner | Compatibility evidence | Status |
|---|---|---|---|---|
| .NET SDK/runtime | TBD | Backend/platform lead | Support lifecycle and library compatibility review | Blocking initialization |
| Android Studio/JBR | TBD | Android lead | Supported IDE/JDK combination | Blocking initialization |
| Kotlin | TBD | Android lead | AGP/Compose or UI-toolkit compatibility | Blocking initialization |
| Android Gradle Plugin/Gradle | TBD | Android lead | Published compatibility matrix | Blocking initialization |
| `minSdk`/`compileSdk`/`targetSdk` | TBD | Product + Android lead | Pilot device matrix and Android policy | Blocking Android initialization |
| PostgreSQL/PostGIS image | PostgreSQL major 16 accepted; exact PostGIS tag/digest TBD | Data/platform lead | Extension and backup compatibility | Blocking Compose initialization |
| Redis image | TBD | Backend/platform lead | Selected .NET queue library compatibility | Blocking worker initialization |
| MinIO image | TBD | Platform lead | Lifecycle/backup/client compatibility | Required only when first-slice object storage is approved |
| Keycloak image | TBD | Security/platform lead | OIDC/client and database compatibility | Required before human-login phase |
| Node.js/pnpm | TBD | Web/platform lead | React toolchain compatibility | Deferred until dashboard initialization |
| Host OS/Docker Desktop/Engine | TBD supported versions | Platform lead | Team host and CI compatibility | Blocking environment sign-off |

## Selection evidence

Record official support lifecycle, compatibility matrix, security update policy, required licenses, container provenance, and a minimal restore/rebuild test. Version pinning is not complete merely because a developer has a version installed locally.

