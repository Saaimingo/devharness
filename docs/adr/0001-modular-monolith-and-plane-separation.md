# ADR-0001: Use a modular monolith with three authority planes

- Status: `APPROVED_DIRECTION_NOT_IMPLEMENTED`
- Date: 2026-08-14
- Owners: project maintainers
- Supersedes: none
- Superseded by: none

## Context

DevHarness needs strong internal ownership boundaries without the deployment, network, consistency, and operational complexity of microservices. It must also prevent ordinary executors and provider integrations from acquiring authority over policy and state promotion.

## Decision

Build the MVP as one Python application organized as a modular monolith. Separate authority conceptually and through dependencies into:

- Control Plane: policy, contracts, orchestration, verification, checkpoints, and memory;
- Execution Plane: disposable sandboxes, executor attempts, and bounded commands;
- Provider Plane: provider authentication, capability discovery, and normalized transport.

Plane separation is an authority boundary, not a requirement for separate processes or network services.

## Alternatives considered

- **Microservices:** rejected for the MVP because distributed failure modes and operations would obscure the thesis being tested.
- **Single undifferentiated package:** rejected because executors and provider code could easily bypass control policy.
- **Plugin-only core:** deferred because stable contracts must exist before extension loading becomes safe.

## Consequences

One process and one release unit simplify development and local operation. Internal dependency rules and contract tests become essential. Future extraction into services remains possible only if evidence justifies it.

## Verification

Phase 0 must define component interfaces and dependency tests showing that domain contracts do not import provider SDKs, execution implementations, CLI composition, or SQLite implementations.

## Revisit triggers

Revisit only when measured isolation, scaling, deployment ownership, or reliability requirements cannot be met inside the modular monolith.
