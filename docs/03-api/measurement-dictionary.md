# First-Slice Measurement Dictionary

Status: **Proposed — RF/domain validation required**

## Rules

- The API field name and unit are canonical; UI labels may differ but must not change meaning.
- Missing radio values are `null`, never zero or a fabricated sentinel.
- Preserve capture provenance and capability so unavailable fields are distinguishable from collection defects.
- Store capture time as a UTC instant. Device-local display time is presentation data.
- Store valid positions in PostGIS using SRID 4326 and longitude/latitude coordinate order when constructing points.
- Do not accept arbitrary `extra` data in the first-slice contract. New measurements require dictionary and contract review.

## Envelope and provenance

| Field | Type/unit | Required | Meaning | Validation status |
|---|---|---:|---|---|
| `observationId` | UUID | Yes | Client-stable identity for one observation | Proposed |
| `capturedAt` | ISO 8601 UTC instant | Yes | Time the observation was captured | Timestamp skew/precision TBD |
| `rat` | Enum | Yes | `GSM`, `WCDMA`, `LTE`, `NR`, or `UNKNOWN` | RF owner review required |
| `capability` | Enum | Yes | `OPEN` or `DEVELOPER` capture capability | Proposed; Diag excluded |
| `servingCellId` | String | No | Normalized serving-cell identity where exposed | Canonical format TBD by RAT |
| `pci` | Integer | No | Physical cell identity where applicable | RAT-specific ranges TBD |
| `arfcn` | Integer | No | Channel number using a RAT-aware interpretation | RAT/band normalization TBD |

## Location

| Field | Type/unit | Required | Meaning | Validation status |
|---|---|---:|---|---|
| `latitude` | Decimal degrees | Yes | WGS84 latitude | `-90..90` proposed |
| `longitude` | Decimal degrees | Yes | WGS84 longitude | `-180..180` proposed |
| `horizontalAccuracyM` | Metres | No | Android-reported horizontal uncertainty | Non-negative; acceptance threshold TBD |
| `altitudeM` | Metres | No | Android-reported altitude relative to its reported reference | Provenance/reference handling TBD |

Invalid or unavailable fixes must not be converted to `(0,0)`. The Android model must keep fix validity and raw provider metadata locally; whether invalid fixes are uploaded as explicitly flagged observations or withheld is a DR-005 decision.

## Radio measurements

| Field | Type/unit | Required | Meaning | Validation status |
|---|---|---:|---|---|
| `rsrpDbm` | dBm | No | Reference signal received power when exposed for the active RAT | RAT/device ranges and normalization TBD |
| `rsrqDb` | dB | No | Reference signal received quality when exposed | RAT/device ranges and normalization TBD |
| `sinrDb` | dB | No | Signal-to-interference-plus-noise ratio when exposed | Android API/source semantics TBD |

The first slice does not claim that every field is available on every device, Android version, operator, RAT, or access capability. The supported-device matrix must state which values are expected, optional, unavailable, or unreliable for each approved pilot device.

## Quality flags requiring final design

Before schema acceptance, the RF/domain and data owners must approve a compact quality model covering at least:

- valid/invalid location fix;
- stale observation or timestamp anomaly;
- API unavailable/permission denied;
- device/OEM value reported as unavailable;
- normalization warning or unknown RAT interpretation;
- mock/test location indication where detectable and legally usable.

## Deferred dictionary areas

GSM/UMTS-specific metrics, NR-specific channel/band details, neighbors, cell-reference matching, throughput, call tests, OTT tests, L3/Diag data, derived KPIs, and commercial reporting fields are outside the first-slice dictionary.

