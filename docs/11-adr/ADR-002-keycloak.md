# ADR-002: Keycloak for Human Identity

- Status: Proposed
- Date: 2026-08-19

## Context

The platform needs human login/SSO and device authentication at very different scales and trust boundaries.

## Decision

Use Keycloak for human OIDC/OAuth2 identity, authentication, sessions, tokens, and coarse roles. Browser clients use Authorization Code with PKCE. RFx APIs retain domain/tenant authorization and RFx-managed device identity. Keycloak data is isolated from RFx application data.

## Consequences

The API supports distinct human and device authentication schemes. A Keycloak subject maps to an RFx profile for tenant/operator/project access. Realm/client layout, claims, MFA, service accounts, and device credential design remain open.

