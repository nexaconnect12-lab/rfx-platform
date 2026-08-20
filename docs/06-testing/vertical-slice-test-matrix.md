# Vertical-Slice Test Matrix

Status: **Draft — device assignments and quantitative thresholds TBD**

| Acceptance ID | Primary test | Automation target | Manual/device evidence | Owner role |
|---|---|---|---|---|
| VS-01 | Valid session through PostGIS | Contract + backend integration + E2E fixture | Real-device capture/database verification | QA + Android + backend |
| VS-02 | Capture while offline | Android instrumentation where feasible | Real-device airplane/network-loss run | Android |
| VS-03 | Reconnect and acknowledge | Android/backend integration harness | Correlated real-device run | Android + backend |
| VS-04 | Identical duplicate batch | API/worker/database integration test | None normally | Backend + data |
| VS-05 | Lost admission response | Network fault-injection E2E test | Representative device confirmation | QA + Android + backend |
| VS-06 | Restart/process recovery | Android instrumentation | Supported-device restart scenario | Android |
| VS-07 | Invalid schema/content | OpenAPI negative contract/fuzz tests | Review safe error text | Backend + security |
| VS-08 | Retry/dead-letter | Worker integration/fault test | Operations status review | Backend + platform |
| VS-09 | Tenant/project isolation | Security integration test matrix | Independent security review | Security + backend |
| VS-10 | Secret/log handling | Repository/log/error automated scans | Secure-storage inspection | Security + Android + platform |
| VS-11 | Geospatial correctness | PostGIS integration test with known coordinates | Known-location comparison | Data + RF |
| VS-12 | Correlation trace | E2E telemetry assertion where feasible | Trace/log evidence review | Platform + QA |

## Release-blocking defect classes

The following block vertical-slice acceptance regardless of numeric coverage targets: measurement loss in an approved ordinary scenario, duplicate domain rows from an identical retry, unauthorized cross-tenant access, credential exposure, false processing completion, untraceable accepted data, inability to revoke a compromised device before pilot, or inability to recover an admitted batch from a tested transient failure.

Coverage percentages, performance thresholds, RF tolerances, supported-device pass counts, and allowable non-critical defects require named-owner approval before they become release gates.

