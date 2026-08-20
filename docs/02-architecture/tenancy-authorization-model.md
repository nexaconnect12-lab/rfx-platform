# Tenancy and Authorization Model

Status: **Proposed — product and security approval required**

## Recommended hierarchy

```text
Organization/Tenant
├── users and memberships
├── devices
└── projects
    └── sessions
        ├── ingestion batches
        └── measurements
```

An operator may be represented as tenant metadata or a separately governed domain entity, but an `operatorTag` string must not be the authorization boundary. Every protected domain row must be reachable through an immutable tenant key.

## First-slice authorization rules

- A device belongs to exactly one tenant for the first slice.
- A device may create/upload only to projects explicitly assigned to it within that tenant.
- A session inherits tenant, project, and device scope at creation; the client cannot later move it across scope.
- Batch and measurement scope is inherited from the authorized session, not accepted from redundant client fields.
- Every database query and mutation applies tenant scope and, where applicable, project/device scope.
- Cross-tenant access fails without revealing whether the target resource exists.
- Administrative overrides are explicit, least-privileged, and audited; none is implied for the first slice.

## Proposed human roles

| Role | Tenant administration | Project membership | Device lifecycle | Session/measurement read | Export | Cross-tenant |
|---|---:|---:|---:|---:|---:|---:|
| `platform-admin` | Exceptional platform operations | Exceptional | Exceptional | Exceptional, audited | Exceptional, audited | Only when explicitly granted |
| `tenant-admin` | Own tenant | Manage | Manage | Own tenant | Own tenant, policy permitting | No |
| `engineer` | No | Assigned projects | View assigned | Assigned projects | Assigned projects, policy permitting | No |
| `viewer` | No | Assigned projects | No | Assigned projects, read-only | No by default | No |

Keycloak supplies authenticated subject and coarse roles. RFx backend membership and resource policies remain authoritative. `platform-admin` should not be enabled for ordinary operational use.

## Required authorization tests

- same-tenant allowed path;
- unassigned-project denial;
- cross-tenant session, batch, measurement, device, and export denial;
- revoked-device denial;
- role escalation and forged-claim denial;
- enumeration-resistant error behavior;
- audit generation for device and administrative changes.

## Decisions still required

- named tenant and initial pilot-project structure;
- whether an RFxPro device can later move tenants and how history is retained;
- exact Keycloak realm/client and role-to-claim mapping;
- whether mobile human login is required after the first slice;
- export permissions and platform-support access approval workflow.

