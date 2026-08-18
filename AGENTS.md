# RFx Platform Agent Instructions

These instructions apply to the entire repository.

## Baseline gate

**Do not generate application source code, executable service scaffolding, database migrations, or deployable infrastructure until a human reviewer explicitly accepts the documentation baseline.** Documentation-only corrections and review changes are allowed before acceptance.

## Source of truth

1. Treat `docs/` and accepted ADRs as the engineering source of truth.
2. Treat source HLD/LLD documents and chat history as inputs, not as authority over accepted repository documentation.
3. If sources conflict, are ambiguous, or show version drift, add or update an open issue in the relevant document. Never silently choose one version.
4. Do not invent requirements, performance targets, security policies, schemas, or contracts. Mark unknowns `TBD` with an owner or decision point.
5. Accepted ADRs are changed only by a superseding ADR; do not rewrite their historical decision.

## Definition of done — strict rule

After **every implementation**, agents must update all relevant documentation before considering the work complete. This includes, as applicable:

- requirements and scope;
- architecture and component responsibilities;
- ADRs for new or changed decisions;
- OpenAPI/API contracts and examples;
- database model, schema documentation, and migrations;
- authentication and authorization rules;
- test strategy, test cases, and verification evidence;
- environment and operational instructions;
- roadmap status, dependencies, and remaining open issues.

Documentation updates must be part of the same change as implementation. “Code complete, docs later” is not permitted. If no documentation changes are needed, the completion note must identify the documents reviewed and explain why they remain accurate.

## Engineering conduct

- Build the RFxPro end-to-end vertical slice before RFx Lite, SDK, or SmartCare/RTP.
- Preserve offline data and idempotency across retries; never trade correctness for convenience.
- Keep human identity (Keycloak) separate from RFx-managed device identity.
- Keep secrets out of source control, logs, Room, and documentation examples.
- Prefer contract-first API and schema changes, backward compatibility, observability, and automated tests.
- Open/Developer capabilities are first; Diag capabilities remain deferred pending access, legality, device support, and product decisions.

