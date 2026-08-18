# ADR-006: Android Shared Core

- Status: Proposed
- Date: 2026-08-19

## Context

RFxPro, RFx Lite, and a future RFx SDK share measurement collection, session, persistence, and synchronization needs, but differ in UX, entitlements, consent, and operating mode.

## Decision

Use Kotlin and build reusable Android core modules for supported RF/location collection, Room persistence, WorkManager synchronization, networking/contracts, session logic, and secure-credential abstractions. Deliver RFxPro first. RFx Lite and the public SDK reuse stable capabilities later rather than being built concurrently.

## Consequences

Public SDK APIs and consent/policy boundaries must be deliberate. Product-specific UI and privileged Diag access do not belong in the shared core. Exact module layout, UI toolkit, Android versions, and map library remain open.

