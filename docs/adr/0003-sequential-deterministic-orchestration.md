# ADR-0003: Begin with sequential deterministic orchestration

- Status: `APPROVED_DIRECTION_NOT_IMPLEMENTED`
- Date: 2026-08-14
- Owners: project maintainers
- Supersedes: none
- Superseded by: none

## Context

Multiple autonomous agents operating concurrently would introduce intermediate states, races, nondeterministic ownership, and difficult recovery before the core safety model has been proven.

## Decision

The MVP orchestrator runs one explicit stage at a time. Every transition has typed inputs, persisted outputs, preconditions, and a terminal outcome. Probabilistic model reasoning is allowed within bounded stages; the surrounding state machine and promotion rules are deterministic.

No stage may silently skip a failed predecessor. Repair loops return to a named earlier state with a new attempt identity.

## Alternatives considered

- **Parallel multi-agent workflow:** deferred until sequential reliability and recovery are measured.
- **Free-form agent loop:** rejected because state and authority would be implicit.
- **Provider-managed orchestration:** rejected because provider portability and Control Plane authority would be lost.

## Consequences

The MVP favors auditability and reproducibility over throughput. Long tasks may take more wall-clock time. Later parallelism must preserve deterministic joins, ownership, and evidence.

## Verification

Tests must cover valid transitions, invalid transitions, interruption and resume, idempotent re-entry where required, repair attempts, and failure without unintended state advancement.

## Revisit triggers

Revisit only after sequential end-to-end scenarios are reliable and measured bottlenecks justify bounded parallel work.
