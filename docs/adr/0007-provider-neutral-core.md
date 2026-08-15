# ADR-0007: Keep provider adapters outside engineering authority

- Status: `APPROVED_DIRECTION_NOT_IMPLEMENTED`
- Date: 2026-08-14
- Owners: project maintainers
- Supersedes: none
- Superseded by: none

## Context

DevHarness must operate with different models and providers without moving project policy into vendor-specific APIs. Provider catalogs, capabilities, costs, and authentication vary and may change independently.

## Decision

Define provider-neutral request, response, capability, usage, and error contracts. Provider adapters handle authentication, catalog discovery where available, capability normalization, transport, and provider error translation.

Model enablement and selection policy belong to the Control Plane. Credentials remain separate from catalogs and persisted workflow data. An adapter cannot alter an Engineering Contract, bypass command policy, approve its output, or promote state.

Phase 0 proves the port with a deterministic fake adapter and at most one thin real adapter only if separately authorized and required for the end-to-end slice.

## Alternatives considered

- **Direct provider SDK calls from components:** rejected because vendor behavior would contaminate core contracts.
- **Provider chooses workflow and model:** rejected because architectural authority would leave the Control Plane.
- **Implement many providers immediately:** rejected because breadth does not prove the harness thesis.

## Consequences

Some provider-specific features may not fit the common capability model and require explicit extensions. The core remains testable without network access or live credentials.

## Verification

Contract tests must run against fake and real adapters where present. Core modules must be importable and testable without provider SDKs or credentials. Tests must prove provider output cannot bypass validation or promotion gates.

## Revisit triggers

Extend the neutral contracts when a concrete, measured provider capability cannot be represented without leaking vendor semantics into the core.
