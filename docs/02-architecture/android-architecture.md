# Android Architecture

Status: **Draft — awaiting baseline acceptance**

## Direction

Use Kotlin and a shared core that supports RFxPro first, then RFx Lite and a future SDK without copying collection or synchronization logic. Product UI, branding, policies, and entitlements remain outside the core.

## Suggested boundaries

- `collection-core`: normalized Open/Developer RF and location observations behind capability adapters.
- `session-core`: session lifecycle and domain models.
- `storage-core`: Room entities/DAOs, migrations, pending-work state.
- `sync-core`: batching, idempotency keys, WorkManager orchestration, retry/backoff.
- `network-core`: typed API contracts and transport concerns.
- `security-core`: interface to Android secure credential storage; no secrets in Room.
- RFxPro application/features: UI, foreground-service lifecycle, maps, NEI/cell references, speed test, exports.

The future SDK must expose a deliberate, consent-aware public surface; it must not simply publish application internals.

## Offline-first invariants

- Persist an observation locally before marking it available for upload.
- Generate stable session and batch identifiers on device.
- Never delete pending data merely because a request was sent; delete/archive only after durable server acknowledgement under an approved retention policy.
- WorkManager retries must be safe and idempotent.
- Foreground collection must remain explicit to the user and comply with Android policies.

## Capability strategy

Implement only APIs available through supported Open/Developer Android access in the first track. Represent unavailable or device-specific measurements explicitly. Diag access is a separate, deferred capability with its own ADR/security/legal review.

## Open issues

- Pin Android Studio, JDK, Kotlin, AGP, Gradle, `minSdk`, `compileSdk`, and `targetSdk` at project initialization.
- Approve supported device/OEM/API matrix and canonical measurement normalization.
- Choose UI framework and map library; prior material mentions MVVM/osmdroid but these are not accepted decisions.
- Define encrypted credential technology because platform APIs evolve.
- Prototype call state/testing separately on target devices.

