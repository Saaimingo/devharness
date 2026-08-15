# Phase 0 / MVP Specification

Status: `APPROVED_DIRECTION_NOT_IMPLEMENTED`

## 1. Mission

Phase 0 builds the smallest end-to-end DevHarness slice capable of accepting a bounded software-change intention, observing a local Git repository, producing an engineering contract, creating an isolated attempt, invoking a controlled executor, independently verifying the result, and recording a validated checkpoint or an explicit rejection.

The objective is to prove the governance loop, not to maximize autonomy, provider breadth, user-interface polish, or task complexity.

## 2. Entry conditions

Implementation may begin only when all of the following are true:

- a maintainer explicitly authorizes Phase 0 implementation;
- naming and repository visibility are finalized;
- the foundation documents and ADRs are reviewed for consistency;
- the implementation task defines exact scope and publication authority;
- a clean or safely isolated starting state is observed and recorded;
- version-dependent assumptions are checked against current official sources;
- no unrelated work will be overwritten or silently included.

This document does not itself authorize implementation, dependency installation, remote creation, branch protection changes, CI configuration, releases, or provider spending.

## 3. Required stack

- Python 3.14
- Pydantic v2
- SQLite
- Git CLI and Git worktrees
- encapsulated subprocess execution
- pytest
- CLI-first interface
- modular monolith
- sequential orchestration
- model-agnostic provider interface

Docker, microservices, vector retrieval, concurrent autonomous agents, and a graphical frontend are not Phase 0 requirements.

## 4. Demonstration scenario

Phase 0 must support a deliberately small fixture repository and a bounded change such as modifying a pure function plus its tests. The scenario must not require production credentials, network access, a live database, or external infrastructure.

The canonical demonstration:

1. initialize or select a controlled fixture repository with a known validated commit;
2. submit a natural-language change request and explicit safety policy;
3. compile versioned Intent IR;
4. capture State IR from exact Git and test evidence;
5. reconcile and version Intent IR if State IR contains a material inherited rule or fact;
6. select a small, local set of relevant engineering rules;
7. compile an Engineering Contract from the applicable Intent IR version;
8. create a Git worktree sandbox;
9. invoke a deterministic fake provider/executor to produce a known change;
10. run independent verification, including diff scope and regression tests;
11. record either a validated checkpoint or a rejection;
12. resume and explain the final state from SQLite without conversation history.

At least one negative scenario must attempt a forbidden command or out-of-scope change and prove that DevHarness rejects it and does not promote the state.

## 5. Phase 0 deliverables

### 5.1 Project and quality skeleton

- installable Python 3.14 package;
- `pyproject.toml` with only justified dependencies;
- CLI entry point;
- unit, contract, integration, and security test layout;
- deterministic test fixtures with no live credentials;
- documented local development commands;
- explicit schema migration mechanism for SQLite.

### 5.2 Versioned contracts

Define Pydantic v2 models for:

- Intent IR;
- State IR;
- Knowledge Item and Knowledge Selection;
- Engineering Contract;
- Sandbox Policy;
- Execution Request and Execution Result;
- Command Request and Command Record;
- Verification Report and evidence references;
- Checkpoint;
- Memory Item and Skill Candidate;
- provider capability, request, response, usage, and error envelopes.

Every persisted contract must include schema version, stable identity, creation time, producer identity, and references to its inputs. Validation errors must be explicit and must not silently coerce safety-critical fields.

### 5.3 Intent Compiler

Minimum capabilities:

- structure objective, context, requirements, constraints, acceptance criteria, exclusions, facts, assumptions, ambiguities, and contradictions;
- distinguish user-provided fact from system inference;
- version intent changes;
- initially use only the human intention plus explicitly supplied persistent memory and known project policy, without inspecting the target repository on its own;
- after State Compilation, produce a new reconciled version when primary repository evidence reveals an inherited rule or fact that materially changes interpretation, safety, architecture, or required behavior;
- link a reconciled version to the prior Intent IR, the triggering State IR evidence, and the reconciliation rationale;
- block on unresolved ambiguity that can materially change safety, architecture, behavior, or outcome;
- never mutate a target repository.

Phase 0 may use a deterministic compiler fixture or a provider-backed implementation behind the neutral port. The contract and validation behavior are mandatory; sophisticated natural-language quality is not.

### 5.4 State Compiler

Capture, when applicable:

- repository root and identity;
- current branch and exact commit;
- working-tree status, staged changes, unstaged changes, and untracked paths;
- relevant files and repository instructions;
- runtime/tool versions;
- available test commands and their observed results;
- ADR and architecture references;
- last validated checkpoint;
- observed risks and unknowns.

The State Compiler must flag discovered rules or facts that may materially affect intent and retain their primary evidence. It does not alter Intent IR itself; the Orchestrator routes the finding back to the Intent Compiler before Engineering Compilation. Findings that are not material remain State IR inputs and do not require a new Intent IR version.

The compiler must not clean, reset, stash, rewrite, or auto-correct the repository. Dirty or ambiguous state is information, not permission to destroy it.

### 5.5 Engineering Knowledge Base

Implement a minimal structured local repository with textual/tag selection. Each record includes:

- type: rule, fact, or heuristic;
- provenance and source identity;
- scope and applicability conditions;
- version/date/validity policy where relevant;
- evidence or confidence;
- exceptions and supersession links.

The selection result must explain why each item was included. Project-specific rules cannot be silently overridden by generic recommendations.

### 5.6 Engineering Compiler

Produce an Engineering Contract containing:

- intent and state input versions;
- selected technical strategy and rationale;
- affected and protected components;
- expected file/diff scope;
- architecture and security invariants;
- allowed and forbidden dependencies and operations;
- sandbox and permission needs;
- test and verification plan;
- rollback strategy;
- risks, assumptions, and unresolved blockers;
- completion criteria.

It must be able to return `BLOCKED` rather than invent missing authority or evidence. It does not write final implementation code.

### 5.7 Orchestrator

Implement the sequential state machine described in the architecture overview. Requirements:

- one active stage per run;
- explicit transition preconditions;
- a conditional, persisted Intent reconciliation transition after State Compilation and before Engineering Compilation when material findings require it;
- persisted inputs, outputs, and errors;
- resumability from a known stage;
- new identity for every execution or repair attempt;
- idempotent handling where a repeated transition is safe;
- no silent advancement after failure;
- no concurrency in the MVP pipeline.

### 5.8 Execution Environment and Command Runner

The default sandbox provider creates a Git worktree from an exact known commit and confines the attempt to it.

The Command Runner must:

- accept executable and argument arrays, not arbitrary shell strings by default;
- validate the resolved working directory against allowed roots;
- use an explicit environment allowlist;
- redact configured secret patterns;
- enforce timeout, cancellation, and bounded capture;
- persist structured command evidence;
- deny force push, destructive Git cleanup/reset, workspace escape, and disallowed executables;
- distinguish read-only, local-write, network, remote-write, and destructive effects;
- route operations requiring approval without pretending they ran.

The initial effect policy must be explicit and deny-by-default:

- local filesystem access is confined to the sandbox, with writes limited to those allowed by the Engineering Contract;
- network access is denied;
- remote-write access is denied;
- production access and mutation are denied;
- destructive effects are denied.

Any elevation requires explicit policy and authority recorded before execution, including the exact capability, target, and scope. An approval route may request that authority, but the operation remains denied until the record exists.

The executor cannot modify Control Plane policy, verification rules, or checkpoint records during an ordinary attempt.

### 5.9 Verification Engine

Evaluate:

- acceptance criteria from Intent IR;
- planned tests and baseline regressions;
- expected versus actual diff scope;
- architecture and security invariants;
- forbidden operations and dependency changes;
- evidence completeness and exact attempt identity.

Verification must run through a component and context separate from the executor attempt. It must inspect primary evidence directly, including the exact diff, Git identity and status, test results, Command Records, and contract-required artifacts. It must not accept the executor's prose summary as proof.

Return a structured verdict: `VALIDATED`, `REPAIR_REQUIRED`, or `REJECTED`. Each conclusion must link primary evidence. The executor cannot write the final Verification Report, choose its final verdict, or promote a checkpoint. The MVP does not require a separate operating-system process or a different model unless a later risk policy requires one.

### 5.10 State/Checkpoint Manager

Persist observed, experimental, and validated states. Only a `VALIDATED` report for the exact attempt and commit may create a trusted checkpoint.

A checkpoint includes:

- checkpoint ID and timestamp;
- repository and exact commit identity;
- intent and engineering contract versions;
- verification report and evidence references;
- satisfied invariants and test summary;
- known limitations and risks;
- next safe step.

Rollback must operate from a known checkpoint and preserve valuable work. Destructive reset is not an acceptable generic rollback implementation.

### 5.11 Persistent Memory + Skills

Persist user, project, and reusable technical memory separately. Implement skill candidates and the four skill classes defined in ADR-0006. Phase 0 does not need autonomous promotion to validated skill; explicit review is acceptable and preferred.

Memory writes must carry provenance and must not store secrets, raw credentials, or unbounded command/model transcripts.

### 5.12 Provider adapters

Define and test the provider-neutral port. A deterministic fake adapter is mandatory for offline, reproducible tests. A live provider adapter is optional and requires separate authorization, credentials, cost controls, official API verification, and redaction tests.

The provider interface reports capabilities; the Orchestrator selects whether and how to use them under Control Plane policy.

## 6. Security test matrix

Phase 0 is incomplete unless automated tests prove at least:

- shell injection is not interpreted as syntax;
- a resolved path cannot escape allowed roots through traversal or links;
- force push is rejected;
- destructive reset/clean commands are rejected;
- network, remote-write, production, and destructive effects remain denied without an exact recorded elevation;
- unrelated tracked and untracked files are preserved;
- inherited secrets are excluded or redacted;
- timeouts terminate the attempt and record the outcome;
- output limits prevent unbounded capture;
- provider output cannot bypass contract validation;
- Engineering Compilation cannot proceed from a stale Intent IR when State IR requires reconciliation;
- executor output cannot self-promote;
- an executor-authored Verification Report or verdict is rejected;
- a failed verification cannot create a trusted checkpoint;
- a checkpoint cannot reference a different attempt or commit;
- protected historical knowledge is not overwritten;
- interruption and resume do not duplicate unsafe effects.

## 7. Acceptance gates

Phase 0 is complete only when all gates are supported by exact evidence:

1. **Contract gate:** all boundary records validate, version, serialize, and reject invalid safety-critical inputs.
2. **State gate:** dirty, clean, detached, and untracked repository states are captured without mutation.
3. **Isolation gate:** a failed attempt leaves the validated checkout unchanged.
4. **Command gate:** forbidden commands and workspace escapes are technically denied.
5. **Verification gate:** scope, regression, acceptance, and invariant evidence produce deterministic verdicts.
6. **Promotion gate:** only the exact independently validated attempt becomes a checkpoint.
7. **Continuity gate:** a new process resumes from SQLite and Git identity without relying on chat context.
8. **Provider gate:** the end-to-end flow passes with a deterministic fake adapter and no network.
9. **Negative-path gate:** at least one malicious or erroneous attempt is rejected without data loss.
10. **Quality gate:** the full relevant pytest suite passes on the supported Python 3.14 environment.
11. **Documentation gate:** actual interfaces, commands, threat boundaries, and limitations match the repository docs and ADRs.
12. **Scope gate:** no Phase 1 feature or unapproved external integration is included.

## 8. Evidence package

The Phase 0 completion review must include:

- exact commit hash and clean/known working-tree status;
- full changed-path inventory;
- test commands, versions, exit codes, and summaries;
- negative security-test results;
- sample redacted Intent IR, State IR, Engineering Contract, Execution Result, Verification Report, and Checkpoint;
- demonstration that the primary checkout was unchanged by a rejected attempt;
- SQLite migration state and integrity result;
- dependency inventory and rationale;
- remaining risks, limitations, and deferred directions.

Do not declare completion from a prose executor summary alone.

## 9. Explicitly deferred

- production-grade autonomous code generation;
- more than one live provider adapter;
- model routing optimization;
- multi-agent or parallel orchestration;
- containers as a universal sandbox;
- vector search;
- graphical interface;
- remote collaboration service;
- production database or infrastructure operations;
- autonomous pull request, merge, release, or deployment;
- automatic skill validation from unreviewed observations;
- self-modification of DevHarness's guardrails.

All deferred items remain `APPROVED_DIRECTION_NOT_IMPLEMENTED` only where an ADR explicitly establishes direction. Otherwise they are simply out of scope and undecided.

## 10. Phase 0 exit decision

After the evidence package is independently reviewed, maintainers may either:

- accept Phase 0 as implemented and record the status change in the relevant ADRs;
- require focused corrections and re-verification;
- reject the design thesis based on evidence;
- authorize a separately specified next phase.

Phase advancement, publication, merge, release, and external deployment are separate decisions and are never implied by passing local tests.
