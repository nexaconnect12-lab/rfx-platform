# Authentication and Authorization

Status: **Draft — awaiting baseline acceptance**

## Identity split

### Humans

Keycloak is the identity provider for administrators, engineers, viewers, and later operator users. Browser clients use OIDC Authorization Code with PKCE. APIs validate issuer, audience, signature, expiry, and required claims. MFA, passwords, login sessions, federation, and token issuance belong to Keycloak.

### Devices

RFx backend services own device registration, credentials, status, rotation/revocation, client type, and access level for RFxPro, Lite, SDK installations, and SmartCare/RTP devices. Devices are not modeled as ordinary human Keycloak users.

The proposed first-slice lifecycle uses an opaque device credential and is detailed in [Device Identity and Credential Lifecycle](device-identity.md).

## Authorization

Keycloak roles provide coarse human roles. The backend remains authoritative for tenant, organization, operator, project, session, device, and data permissions. A Keycloak subject maps to an RFx user profile; every resource query must apply the relevant tenancy scope.

The recommended hierarchy and initial role matrix are defined in [Tenancy and Authorization Model](tenancy-authorization-model.md). Role names and support-access policy remain subject to product/security approval.

## Security rules

- Use TLS in every non-local environment.
- Do not log credentials or full tokens.
- Store Android credentials with OS-backed secure storage, not Room.
- Keep Keycloak data isolated from RFx application schemas/databases.
- Audit sensitive administrative, export, device, and authorization changes.
- Define revocation and compromised-device response before pilots.

## Open issues

- Approve the proposed opaque device credential and bootstrap/rotation/recovery details.
- Keycloak realm/client topology, audience model, role/claim mapping, MFA, and service identities.
- Approve the proposed tenant/project hierarchy and cross-tenant support-access policy.
- Whether mobile human login is needed in addition to device identity.
- Data-export authorization and audit retention.
