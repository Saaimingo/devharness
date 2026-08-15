# ADR-0002: Adopt the Phase 0 technology baseline

- Status: `APPROVED_DIRECTION_NOT_IMPLEMENTED`
- Date: 2026-08-14
- Owners: project maintainers
- Supersedes: none
- Superseded by: none

## Context

The MVP requires strong typed contracts, reliable local persistence, controlled process execution, Git-native isolation, and a low-friction interface. It does not require a distributed platform or graphical application.

## Decision

Use:

- Python 3.14 as the runtime baseline;
- Pydantic v2 for validated inter-component and boundary contracts;
- SQLite for local metadata persistence;
- Git CLI and worktrees for source identity and default isolation;
- the Python subprocess facilities only through an encapsulated Command Runner;
- pytest for unit, integration, contract, and guardrail tests;
- a CLI-first interface.

Version-dependent details must be verified against current official documentation when implementation begins. Dependencies beyond this baseline require evidence and an explicit decision.

## Alternatives considered

- **Older Python baseline:** rejected because this is a new project and the approved target is Python 3.14.
- **PostgreSQL:** deferred because a local, single-user control plane does not initially require a server database.
- **Direct shell invocation throughout modules:** rejected because it prevents consistent policy, audit, timeout, and redaction enforcement.
- **GUI-first:** rejected because it would delay proof of the engineering pipeline.

## Consequences

The project gains a compact local stack and strong structured contracts. Python 3.14 ecosystem compatibility must be checked before dependency selection. SQLite concurrency and subprocess portability require explicit tests.

## Verification

Phase 0 must demonstrate the supported Python version, Pydantic v2 contract validation, transactional SQLite persistence, Git worktree lifecycle, policy-enforced command execution, and pytest coverage.

## Revisit triggers

Revisit when measured concurrency, deployment, platform support, or ecosystem constraints invalidate a baseline choice.
