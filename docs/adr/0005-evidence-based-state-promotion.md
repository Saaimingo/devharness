# ADR-0005: Promote only independently verified states

- Status: `APPROVED_DIRECTION_NOT_IMPLEMENTED`
- Date: 2026-08-14
- Owners: project maintainers
- Supersedes: none
- Superseded by: none

## Context

An implementation may appear functional while introducing regressions, security risks, architectural violations, or scope drift. A commit alone does not explain why a state is trusted.

## Decision

Classify states as observed, experimental, or validated. Only an experimental state that passes independent Verification Engine gates may become validated and produce a trusted checkpoint. Executors cannot approve or promote their own results.

A checkpoint records exact Git identity, Intent IR and Engineering Contract versions, verification evidence, satisfied invariants, known risks and limitations, and the next safe step. Rollback restores from known evidence without silently destroying valuable work.

## Alternatives considered

- **Commit equals checkpoint:** rejected because Git identity is not acceptance evidence.
- **Executor self-verification:** rejected because it does not provide independent judgment.
- **Conversation history as continuity:** rejected because it is transient and not an executable state record.

## Consequences

Promotion requires explicit evidence and may be slower than direct agent workflows. The project gains reproducible trust decisions, resumability, and safe repair or rejection paths.

## Verification

Tests must prove that failed, incomplete, missing, or self-authored verification cannot create a trusted checkpoint; that exact evidence is linked; and that rollback preserves unrelated or valuable work.

## Revisit triggers

The independence mechanism may evolve, but no revision may remove evidence-backed promotion or allow the executor to be sole judge.
