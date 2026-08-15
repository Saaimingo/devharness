# Planned Repository Layout

Status: `APPROVED_DIRECTION_NOT_IMPLEMENTED`

This document reserves ownership boundaries for Phase 0. It does not authorize creating or implementing these packages before the Phase 0 gate is opened.

```text
devharness/
├── src/devharness/
│   ├── cli/                 # CLI composition; no domain policy
│   ├── contracts/           # Versioned Pydantic IRs and envelopes
│   ├── intent/              # Intent Compiler
│   ├── state/               # State Compiler
│   ├── engineering/         # Engineering Compiler
│   ├── knowledge/           # Knowledge selection and provenance
│   ├── orchestration/       # Sequential state machine
│   ├── execution/           # Sandbox and Command Runner abstractions
│   ├── verification/        # Evidence evaluation and verdicts
│   ├── checkpoints/         # Validated-state lifecycle
│   ├── memory/              # Durable memory and skill lifecycle
│   ├── providers/           # Provider-neutral interfaces and adapters
│   └── persistence/         # SQLite repositories and migrations
├── tests/
│   ├── unit/
│   ├── integration/
│   ├── contracts/
│   └── security/
├── docs/
│   ├── adr/
│   ├── architecture/
│   └── specifications/
├── AGENTS.md
├── CONTRIBUTING.md
├── SECURITY.md
└── pyproject.toml
```

## Dependency direction

- Domain contracts must not import provider SDKs, CLI frameworks, or persistence implementations.
- CLI composes use cases but does not contain business policy.
- Provider adapters depend on provider-neutral ports, never the reverse.
- Execution implementations depend on policy interfaces owned by the Control Plane.
- Persistence implementations satisfy repositories defined by owning domain modules.
- Verification consumes immutable execution evidence and cannot mutate an execution attempt.

The exact package names may be refined during Phase 0 only through an ADR-compatible change.
