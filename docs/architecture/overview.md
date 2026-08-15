# DevHarness Architecture

Status: `APPROVED_DIRECTION_NOT_IMPLEMENTED`

## 1. Purpose

DevHarness is a model-agnostic software engineering harness. It converts a human intention into a structured, isolated, independently verified, and resumable software change. Its job is not to make a language model infallible. Its job is to constrain failure, preserve evidence, and keep accepted state recoverable.

The MVP must prove this thesis:

> A human intention can pass through an automated engineering, implementation, and verification process with fewer regressions, less out-of-scope change, and safer state handling than a task handed directly to a coding agent.

## 2. Architectural principles

1. **Human owns intent; harness owns engineering controls.**
2. **Observed is not validated.** Repository state is described before it is trusted.
3. **Executor is not the final judge.** Promotion requires independent verification.
4. **Errors must be limited, detectable, traceable, and reversible.**
5. **Guardrails must be technical where possible.** Prompts alone are not authorization boundaries.
6. **Workflow is deterministic.** Probabilistic reasoning is contained inside explicit stages.
7. **Knowledge is selective and sourced.** Rules, facts, and heuristics remain distinguishable.
8. **Provider is transport, not authority.** The core must remain model-agnostic.
9. **No experiment runs on the only valid state.** Isolation scales with blast radius.
10. **Continuity starts from persisted state, not chat memory.**

## 3. Planes and authority

### 3.1 Control Plane

The Control Plane owns policy and state transitions. It contains:

- Intent Compiler;
- State Compiler;
- Engineering Compiler;
- Engineering Knowledge Base;
- Orchestrator;
- Verification Engine;
- State/Checkpoint Manager;
- Persistent Memory + Skills;
- policy evaluation and approval gates.

Ordinary execution tasks must not be able to rewrite the controls that constrain them.

### 3.2 Execution Plane

The Execution Plane contains disposable environments and bounded command execution:

- Git worktree sandbox provider;
- task-specific filesystem and virtual environment;
- encapsulated Command Runner;
- executor session;
- captured outputs and artifacts.

It operates with least privilege. A failed attempt remains experimental and may be archived or discarded without altering the last validated state.

### 3.3 Provider Plane

The Provider Plane contains normalized adapters for models and external execution providers. An adapter may:

- authenticate through an approved secret boundary;
- discover models and capabilities;
- normalize requests and responses;
- report cost, availability, limits, and tool support when known.

An adapter may not select architecture, reinterpret project policy, promote state, or bypass an approval gate.

## 4. Core components

### 4.1 Intent Compiler

Transforms natural-language input into a versioned **Intent IR**. It identifies goals, requirements, constraints, acceptance criteria, exclusions, facts, assumptions, ambiguities, contradictions, and inherited project rules. It does not inspect or mutate the project beyond the inputs explicitly supplied to it.

### 4.2 State Compiler

Builds a **State IR** from observed technical evidence: repository identity, branch, commit, working tree, untracked files, runtime, dependencies, tests, architecture records, known risks, and last validated checkpoint. It preserves existing work and distinguishes an observed state from a proven healthy state.

### 4.3 Engineering Knowledge Base

Returns only knowledge relevant to the current intent and state. Each important item carries type (`rule`, `fact`, or `heuristic`), provenance, scope, version or temporal validity, confidence or evidence, and known exceptions. It does not mutate the target project.

### 4.4 Engineering Compiler

Combines Intent IR, State IR, and selected knowledge into an **Engineering Contract**. The contract defines affected components, technical strategy, constraints, risks, allowed and forbidden dependencies, verification requirements, rollback strategy, expected scope, and completion gates. It may block execution when evidence or authority is insufficient.

### 4.5 Orchestrator

Runs the state machine and enforces transition preconditions. It does not replace component ownership. In the MVP it invokes one stage at a time, persists stage outputs, and resumes only from a known transition.

### 4.6 Execution Environment and Command Runner

Creates isolation proportional to risk. Git worktrees are the default source-code sandbox. The Command Runner is the only subprocess boundary and owns argument handling, working-directory validation, environment filtering, timeouts, output limits, cancellation, audit records, and command policy decisions.

### 4.7 Verification Engine

Evaluates the implementation attempt against functional acceptance criteria, regression checks, architectural invariants, security requirements, planned scope, and the Engineering Contract. It produces a **Verification Report** with evidence. It is independent of the executor and cannot accept unverified claims as proof.

### 4.8 State/Checkpoint Manager

Tracks three state classes:

- **observed** — captured but not proven healthy;
- **experimental** — an isolated implementation attempt;
- **validated** — independently verified and eligible to become a trusted checkpoint.

A checkpoint records commit identity, intent version, verification evidence, invariants, decisions, risks, limitations, and next step. Rollback is controlled restoration, not destructive reset.

### 4.9 Persistent Memory + Skills

Stores durable knowledge separately from executable state. Memory classes are user, project, and reusable technical memory. Skills are classified as technical-evolving, stable-engineering, project-specific, or protected-historical. Historical records are superseded, not silently overwritten. A single successful observation does not automatically become a validated skill.

### 4.10 Provider Adapters

Expose a common capability interface so the Control Plane does not depend on a vendor-specific API. Provider catalog and credentials remain separate from enabled-model policy.

## 5. Sequential lifecycle

```text
RECEIVED
  -> INTENT_COMPILED
  -> STATE_COMPILED
  -> ENGINEERING_CONTRACT_READY
  -> SANDBOX_READY
  -> EXECUTION_COMPLETE
  -> VERIFICATION_COMPLETE
       -> REPAIR_REQUIRED -> EXECUTION_COMPLETE
       -> REJECTED
       -> VALIDATED
  -> CHECKPOINT_RECORDED
  -> KNOWLEDGE_CANDIDATES_RECORDED
```

Every transition requires a persisted input identifier, output identifier, timestamp, actor/provider identity, and outcome. Failures do not silently advance the state machine.

## 6. Conceptual contracts

Phase 0 will formalize these Pydantic v2 contracts:

| Contract | Produced by | Consumed by | Minimum identity |
| --- | --- | --- | --- |
| Intent IR | Intent Compiler | Engineering Compiler, Verification Engine | intent ID + version |
| State IR | State Compiler | Engineering Compiler, Checkpoint Manager | repository + commit + capture ID |
| Knowledge Selection | Knowledge Base | Engineering Compiler | query/context ID + sources |
| Engineering Contract | Engineering Compiler | Orchestrator, Executor, Verification | contract ID + input IDs |
| Execution Request | Orchestrator | Execution Plane | contract ID + sandbox policy |
| Execution Result | Execution Plane | Verification Engine | attempt ID + artifacts + command records |
| Verification Report | Verification Engine | Checkpoint Manager | attempt ID + evidence + verdict |
| Checkpoint | Checkpoint Manager | future State Compiler | checkpoint ID + verified commit |
| Memory/Skill Candidate | Control Plane | Memory store/review flow | provenance + scope + status |

Contract payloads must be versioned. Large logs and artifacts should be referenced by content identity rather than duplicated into every record.

## 7. Persistence boundaries

SQLite is the local source of truth for workflow metadata, contract metadata, approvals, checkpoints, memory records, and audit indexes. Git remains the source of truth for source history and worktree identity. Artifact bytes may live in a controlled filesystem store and be referenced by checksum from SQLite.

Database mutations must be transactional. Schema evolution must use explicit migrations once implementation begins. Secrets and raw provider credentials must never be persisted in ordinary workflow records.

## 8. Initial physical architecture

The implementation is planned as one installable Python application with internal modules aligned to component ownership. No internal component communicates through a network merely to simulate service separation. Boundaries are enforced through interfaces, contracts, dependency direction, and tests.

Planned toolchain:

- Python 3.14;
- Pydantic v2;
- SQLite;
- Git CLI and worktrees;
- encapsulated `subprocess` execution;
- pytest;
- CLI-first interface.

## 9. MVP non-goals

- graphical user interface or heavy frontend;
- mandatory Docker or container platform;
- microservices;
- concurrent autonomous agents;
- vector database or large generic RAG system;
- production deployment automation;
- marketplace of skills;
- broad provider catalog;
- self-modifying guardrails;
- autonomous merge, release, or production mutation.

## 10. Trust model

Model output, executor claims, command output, repository documentation, external knowledge, and remembered facts are inputs with different trust levels. Acceptance must depend on primary evidence appropriate to the claim: structured contracts, exact repository state, command records, diffs, checksums, test results, and independently evaluated policies.
