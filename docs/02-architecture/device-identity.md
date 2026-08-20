# Device Identity and Credential Lifecycle

Status: **Proposed — security approval required**

## Recommended first-slice model

RFx owns device identity independently from Keycloak human identity. A device identity represents one RFx application installation, not a person, SIM, handset forever, or Keycloak user.

Use a high-entropy opaque bearer credential for the first slice. Return it only at bootstrap/rotation, store only a one-way verifier or keyed hash server-side, and transmit it only over TLS. This avoids putting long-lived authorization claims into a self-contained token before revocation, tenancy, and clock behavior are proven.

## Lifecycle

1. **Bootstrap:** an authorized operator/admin creates or supplies a single-use, short-lived bootstrap credential scoped to the intended tenant/project and RFxPro client type. Exact issuance UI/process and expiry are TBD.
2. **Registration:** the app submits installation and device metadata. The backend creates a device record and returns the opaque credential once.
3. **Secure storage:** Android stores the credential behind an OS-backed secure-storage abstraction. It must not be stored in Room, preferences without approved encryption, logs, crash reports, backups, screenshots, or documentation examples.
4. **Use:** the device presents the credential to RFx APIs. Authorization is evaluated from current server-side device status and scope, not trusted client claims.
5. **Rotation:** the server issues a new credential, permits a bounded overlap if approved, records the event, and revokes the old credential.
6. **Revocation:** an administrator or automated security control can immediately disable a credential/device. APIs reject further use and audit the reason.
7. **Recovery:** loss of secure storage requires re-bootstrap; the server never reveals the prior credential.
8. **Retirement:** retiring a device revokes credentials while preserving required audit/session ownership records under the retention policy.

## Required server-side metadata

- device ID and installation ID;
- tenant and permitted project scope;
- client type and approved access capability;
- credential identifier/verifier, creation, last-used, expiry if adopted, rotation, and revocation data;
- device status such as `PENDING`, `ACTIVE`, `SUSPENDED`, `REVOKED`, `RETIRED`;
- non-secret device/app metadata needed for support and capability validation;
- audit actor, reason, and correlation identifiers for lifecycle changes.

## Compromise response

Before pilot use, document and test: immediate revocation, affected-device identification, token/log exposure review, credential re-bootstrap, suspicious batch review, user/customer notification decision, and evidence preservation.

## Human decisions still required

- named authority permitted to create bootstrap credentials;
- bootstrap delivery channel, expiry, and attempt limits;
- device credential expiry versus rotation-only policy;
- rotation overlap period and offline-device behavior;
- lost/replaced handset workflow;
- whether hardware/app attestation is required later;
- audit and credential-metadata retention.

