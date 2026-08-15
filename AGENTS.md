# AGENTS.md

This file defines the operating contract for human and automated contributors to DevHarness.

## Current project state

The repository is in **foundation-only** status. Architecture and Phase 0 directions are documented but not implemented. Until an implementation task is explicitly authorized, do not add application code, dependency manifests, generated lockfiles, database schemas, executable entry points, CI workflows, or provider integrations.

For approved future directions that are not implemented, use the exact status:

`APPROVED_DIRECTION_NOT_IMPLEMENTED`

Do not shorten that status to `APPROVED`.

## Authority and precedence

Follow instructions in this order:

1. explicit user or maintainer instructions for the current task;
2. this file and any more specific `AGENTS.md` in the affected subtree;
3. accepted ADRs and project specifications;
4. other repository documentation.

If instructions conflict materially, stop before changing state and ask for a decision. Do not silently reinterpret an architectural or security boundary.

## Mandatory workflow

1. Inspect repository, branch, commit, working tree, untracked files, and applicable instructions before editing.
2. Separate observed facts from assumptions and inferences.
3. Preserve unrelated changes. Never clean, reset, overwrite, or move them merely to obtain a clean workspace.
4. Keep the change as small and reversible as the requirement allows.
5. Record architectural changes with an ADR before implementation.
6. Validate the exact diff and run checks proportionate to the change.
7. Report what was observed, changed, verified, and left pending.

## Safety boundaries

- Never use force push.
- Never run destructive Git operations such as `reset --hard`, destructive checkout, or unscoped clean commands.
- Never delete or overwrite user data, untracked files, credentials, backups, or forensic material.
- Never expose secrets in source, logs, fixtures, commits, prompts, or issue content.
- Never treat production systems, credentials, databases, or infrastructure as an experiment environment.
- Never grant an executor permission to modify the control mechanism that constrains it during ordinary execution.
- Never allow an executor to promote its own result to a validated checkpoint.
- Never infer authorization to publish, merge, tag, release, alter repository settings, advance phases, or expand scope.

Actions with material external effects require the exact target, a reversible plan where possible, and explicit authorization when that action was not already requested.

## Architecture invariants

- DevHarness remains model-agnostic. Provider-specific behavior stays behind adapters.
- Provider adapters are transport and capability normalization, not engineering authority.
- Control Plane, Execution Plane, and Provider Plane boundaries remain explicit.
- The MVP is a CLI-first modular monolith.
- The orchestration flow is sequential and state transitions are explicit.
- Probabilistic reasoning may occur inside bounded stages; workflow and promotion rules remain deterministic.
- Intent Compiler and State Compiler observe and structure; they do not implement changes.
- The initial Intent Compiler uses supplied intent, memory, and known policy; material repository findings require a versioned Intent IR reconciliation before engineering compilation.
- Engineering Compiler produces an Engineering Contract; it does not implement the final code.
- The Engineering Knowledge Base retrieves and classifies knowledge; it does not mutate projects.
- Execution occurs in a disposable, least-privilege sandbox proportional to blast radius.
- Filesystem writes are allowed only inside the contracted sandbox; network, remote-write, production, and destructive effects are denied by default and require explicit, recorded policy and authority to elevate.
- Only independently verified states may become trusted checkpoints.
- Verification uses a separate component and context, reads primary evidence directly, and never accepts an executor-authored final verdict or checkpoint promotion.
- Rollback must preserve valuable work and must not mean silent destruction.
- Memory, skills, and checkpoints are distinct concepts with provenance and validity metadata.

## Phase 0 implementation gate

When Phase 0 implementation is explicitly authorized, first confirm that the task is consistent with:

- `docs/specifications/phase-0-mvp.md`;
- all records in `docs/adr/`;
- the public interfaces and ownership boundaries in `docs/architecture/overview.md`.

Any proposed deviation must be documented as a new ADR or an amendment that supersedes an existing ADR. Do not implement first and document later.

## Quality expectations

Once code exists:

- target Python 3.14;
- use Pydantic v2 for validated external and inter-component contracts;
- use type annotations for public interfaces;
- add focused pytest coverage for behavior and security guardrails;
- include failure-path and forbidden-operation tests;
- encapsulate all subprocess execution behind the Command Runner boundary;
- keep SQLite access behind a persistence interface and use explicit migrations;
- avoid optional infrastructure in the MVP unless an ADR authorizes it.

## Commit discipline

- Use small, coherent commits with imperative, descriptive subjects.
- Stage only files belonging to the current change.
- Do not combine formatting churn, unrelated refactors, generated files, or dependency updates with a focused change.
- Before committing, inspect staged paths and staged diff; after committing, verify the recorded commit and working-tree state.

## Review checklist

- Does the change remain inside authorized scope?
- Are facts, assumptions, risks, and unimplemented directions labeled accurately?
- Are plane and component authority boundaries preserved?
- Is the result reversible, or is the irreversibility explicitly approved and protected?
- Can independent evidence verify every acceptance claim?
- Are secrets, user data, existing work, and remote state protected?
- Are docs and ADRs consistent with the actual change?
