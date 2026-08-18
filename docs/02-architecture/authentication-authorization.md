# Authentication and Authorization

Status: **Draft — awaiting baseline acceptance**

## Identity split

### Humans

Keycloak is the identity provider for administrators, engineers, viewers, and later operator users. Browser clients use OIDC Authorization Code with PKCE. APIs validate issuer, audience, signature, expiry, and required claims. MFA, passwords, login sessions, federation, and token issuance belong to Keycloak.

### Devices

RFx backend services own device registration, credentials, status, rotation/revocation, client type, and access level for RFxPro, Lite, SDK installations, and SmartCare/RTP devices. Devices are not modeled as ordinary human Keycloak users.

## Authorization

Keycloak roles provide coarse human roles. The backend remains authoritative for tenant, organization, operator, project, session, device, and data permissions. A Keycloak subject maps to an RFx user profile; every resource query must apply the relevant tenancy scope.

Provisional roles: `admin`, `engineer`, `viewer`, and `operator-admin`. The exact permission matrix is unresolved.

## Security rules

- Use TLS in every non-local environment.
- Do not log credentials or full tokens.
- Store Android credentials with OS-backed secure storage, not Room.
- Keep Keycloak data isolated from RFx application schemas/databases.
- Audit sensitive administrative, export, device, and authorization changes.
- Define revocation and compromised-device response before pilots.

## Open issues

- Device credential format: opaque token versus separately issued JWT; rotation/bootstrap/recovery flows.
- Keycloak realm/client topology, audience model, role/claim mapping, MFA, and service identities.
- Tenant model and cross-tenant administrative access.
- Whether mobile human login is needed in addition to device identity.
- Data-export authorization and audit retention.

