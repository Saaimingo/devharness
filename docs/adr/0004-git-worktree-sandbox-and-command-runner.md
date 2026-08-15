# ADR-0004: Use Git worktrees and an encapsulated Command Runner

- Status: `APPROVED_DIRECTION_NOT_IMPLEMENTED`
- Date: 2026-08-14
- Owners: project maintainers
- Supersedes: none
- Superseded by: none

## Context

An executor may misunderstand a task or run an unsafe command even when upstream planning is correct. The only valid project state must not be the experiment surface, and command policy cannot be scattered across executors.

## Decision

Use Git worktrees as the default source-code sandbox for the MVP. Scale isolation with risk using task-specific directories, virtual environments, copied data, limited credentials, mocks, or disposable resources when needed. Docker is not mandatory.

All subprocess execution passes through one Command Runner boundary responsible for:

- executable and argument policy;
- resolved working-directory containment;
- explicit environment allowlisting and secret redaction;
- timeouts, cancellation, and output limits;
- structured stdout, stderr, exit status, and timing records;
- denial or approval routing for destructive and external-effect commands.

Shell-string execution is denied by default. Force push and destructive cleanup are not normal operations.

## Alternatives considered

- **Direct execution in the primary checkout:** rejected because failures could corrupt the only valid state.
- **Docker required for every task:** deferred because it adds platform and operational cost beyond the MVP's default needs.
- **Prompt-only command safety:** rejected because prompts are not enforceable permission boundaries.

## Consequences

Worktree lifecycle and platform behavior become core responsibilities. Not all non-file side effects are isolated by a worktree, so the risk classifier and external resource policy remain necessary.

## Verification

Security tests must prove rejection of forbidden executables, shell injection attempts, workspace escapes, disallowed environment inheritance, force push, destructive Git cleanup, timeout overrun, and oversized output. Integration tests must prove creation and disposal without changing the validated checkout.

## Revisit triggers

Revisit when supported workloads require stronger filesystem, process, network, or operating-system isolation than worktrees and least-privilege processes can provide.
