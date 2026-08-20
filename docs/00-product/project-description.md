# RFx Platform Project Description

Status: **Draft — awaiting baseline acceptance**

## Summary

RFx Platform is a self-owned mobile-network measurement and quality-analysis platform. It begins with **RFxPro**, a professional Android field-engineering product that collects available radio-frequency and location observations, preserves them safely while offline, synchronizes them reliably, and makes them available for secure geospatial storage and later engineering analysis.

The platform is designed for field conditions where connectivity may be weak or unavailable. Offline operation, loss prevention, safe retry, duplicate prevention, tenant isolation, and end-to-end traceability are therefore core product behaviors rather than optional technical enhancements.

## Product purpose

RFxPro enables a field engineer to perform a measurement session using a supported Android device. Depending on the device, modem, Android version, permissions, radio access technology, and approved access capability, the application may capture:

- GPS location, time, and reported accuracy;
- radio access technology such as GSM, WCDMA, LTE, or 5G NR;
- serving-cell identity and available channel/cell identifiers;
- RSRP, RSRQ, SINR, and other approved measurements when exposed by the platform;
- capture provenance and capability information needed to interpret missing or device-specific values.

Unavailable values are represented explicitly. RFx Platform does not fabricate a value merely because an Android API, modem, permission, or device cannot provide it.

## First delivery

The first delivery is an end-to-end RFxPro vertical slice:

```text
RFxPro Android capture
    -> Room local persistence
    -> WorkManager synchronization
    -> ASP.NET Core API
    -> durable batch admission and PostgreSQL outbox
    -> Redis-backed work coordination
    -> .NET Worker processing
    -> PostgreSQL/PostGIS
```

The slice proves that an approved real Android device can capture the minimum RF/GPS observation set, continue through ordinary offline conditions, recover pending work, upload safely, avoid duplication under retry, and produce an authorized and traceable geospatial record in PostGIS.

The first delivery prioritizes correctness and recoverability over feature breadth. Maps, reports, advanced analytics, commercial packaging, and additional products do not replace this reliability gate.

## Offline and ingestion model

An observation is persisted locally before becoming eligible for upload. The Android client creates stable session and batch identifiers. The API returns `202 Accepted` only after it has durably recorded the admitted batch, its authorization scope, payload identity, and idempotency state.

If a response is lost and Android retries the same batch, the backend returns the existing semantic result and does not create duplicate measurements. Redis coordinates asynchronous work but is never the sole authoritative copy of accepted data. PostgreSQL admission/outbox records and idempotent .NET workers protect the workflow across transient failures and repeated delivery.

## Identity and authorization

Human identity and device identity are separate trust domains:

- **Keycloak** authenticates human administrators, engineers, and viewers.
- **RFx backend services** manage device registration, credentials, rotation, revocation, status, and assigned scope.
- **RFx authorization policies** enforce tenant, project, device, session, batch, measurement, and export permissions.

For the proposed first slice, a device belongs to one tenant and may operate only in assigned projects. Scope is derived from authorized server-side relationships, not trusted from client-supplied tags. Android credentials use an approved OS-backed secure-storage mechanism and are never stored in Room, logs, source control, or documentation examples.

## Data and service responsibilities

- **PostgreSQL 16/PostGIS** is the authoritative operational and geospatial system of record.
- **ASP.NET Core** is the accepted main backend system for APIs, validation, authorization, durable admission, and queries.
- **.NET Worker Services** process admitted batches and other asynchronous work.
- **Redis** provides transient queue and coordination capabilities, not durable business truth.
- **MinIO** stores approved large immutable objects and exports where required; PostgreSQL retains ownership and lifecycle metadata.
- **Keycloak** owns human authentication data and token issuance; it does not own RFx domain authorization or device identity.

The principal first-slice domain concepts are tenant, project, device, session, session-state history, ingestion batch, batch attempt, measurement, audit event, and optional object metadata.

## Subsequent RFxPro capabilities

After the vertical slice is accepted, subsequent RFxPro increments may add:

- session UI, history, and recovery controls;
- map-based inspection;
- cell-reference imports and serving-cell matching;
- speed testing;
- CSV, KML, PDF, and other approved exports;
- Keycloak-backed human access;
- React/Leaflet dashboard functionality;
- GeoServer layers and MapProxy/OSM basemap reuse;
- reports, audit workflows, performance hardening, and production operations.

Each increment must preserve the accepted offline, idempotency, authorization, privacy, observability, and documentation requirements.

## Future platform products

The shared platform may later support:

- **RFx Lite** for a lightweight collection experience;
- **RFx SDK** for consent-aware embedding in an operator application;
- advanced and busy-hour analytics;
- **RFx SmartCare/RTP** fixed-site monitoring;
- enterprise and regulator-oriented reporting where separately approved;
- **Diag/L3** collection on controlled specialist devices.

These capabilities are outside the first delivery. Lite and SDK require separate consent/privacy design; SmartCare/RTP requires hardware and operational planning; Diag/L3 requires device-access, security, legal, and product approval. Future architecture must build on proven RFxPro foundations rather than expanding the initial commitment speculatively.

## Business value

RFx Platform shortens the path between field collection and engineering analysis while keeping the organization in control of its software and data. A field engineer captures measurements, the platform preserves and synchronizes them without manual log transfer or duplicate processing, and authorized office engineers can inspect the resulting coverage and quality information through later analysis capabilities.

The initial value proposition is reliable ownership of the complete measurement workflow. The project does not depend on delivering every telecom testing capability on day one; it first proves that measurement data can be captured, retained, transferred, authorized, processed, and queried correctly.

## Project statement

**RFx Platform is an offline-first, tenant-secure, and traceable RF measurement platform. RFxPro is its first professional Android client, used to prove the foundation from real-device capture to PostGIS before the platform expands into visualization, analytics, embedded collection, and enterprise monitoring.**

