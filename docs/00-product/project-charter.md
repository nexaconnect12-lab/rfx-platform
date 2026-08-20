# Project Charter

Status: **Draft — awaiting baseline acceptance**

## Purpose

RFx Platform provides a shared foundation for collecting RF and location measurements on Android devices, retaining them safely while offline, synchronizing them reliably, processing them asynchronously, storing/querying them geospatially, and presenting them to engineers and operators.

## Outcomes

- Deliver an RFxPro vertical slice from device capture through PostGIS verification.
- Establish reusable Android collection and synchronization capabilities for later RFx Lite and RFx SDK products.
- Establish secure human and device access models with clear ownership boundaries.
- Enable map-based inspection and reporting without duplicating existing GeoServer, MapProxy, or OSM capabilities.
- Maintain documentation as a living engineering source of truth.

## Principles

- Offline-first and loss-resistant.
- Contract-first and idempotent.
- Modular monolith/API plus workers before unnecessary microservices.
- Open/Developer-accessible Android capabilities first; privileged Diag access deferred.
- Product expansion follows proven RFxPro foundations.
- Security, privacy, tenancy, and observability are designed in, not added at release time.

## Stakeholders

Product owner, RF/domain engineers, Android engineers, backend/data engineers, web engineers, platform/operations engineers, security/privacy reviewers, and pilot operator/customer representatives.

## Success indicators

Quantitative targets remain to be approved. Initial acceptance must demonstrate a real-device RFxPro session that continues offline, retries safely, produces no duplicates, reaches PostGIS, and can be inspected end to end.

## Constraints

- Android RF values and call behavior vary by device, modem, OEM, Android version, permissions, and network.
- OSM data/tile usage must comply with attribution, licensing, and service policies.
- Diag/privileged collection may require vendor agreements, specialized hardware, or legal approval.
- The baseline must be accepted before source generation.

## Open issues

- Define named approver(s) and the explicit baseline-acceptance record.
- Approve measurable product/service SLOs, supported countries/operators, privacy retention, and pilot device matrix.
- Reconcile terminology and version differences across the source HLD and LLD. RFxPro LLD v1.6 is the priority first-delivery input and HLD v2.1 primarily informs future work; neither overrides accepted ADR-001's .NET backend decision.
