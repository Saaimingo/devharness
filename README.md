# DevHarness

**A model-agnostic software engineering harness for safe, traceable, and governed development.**

> Project status: **foundation only**. The harness is not implemented yet.

DevHarness is intended to transform human intent into verified software changes through a controlled engineering workflow. It is not a chatbot, a general-purpose IDE, or a model provider. Models are replaceable execution engines; DevHarness owns the contracts, policies, isolation, verification, checkpoints, and durable context around them.

## Why DevHarness

AI coding systems are probabilistic and will sometimes misunderstand intent, produce incorrect code, exceed scope, or propose destructive actions. DevHarness is designed around a different assumption: errors cannot always be prevented, but they should be limited, detectable, traceable, and reversible.

The constitutional rule is:

> No automated action may create an irreversible loss greater than the system can safely restore.

## Target workflow

```text
Human intent
    -> Initial Intent Compiler
    -> State Compiler
    -> Intent reconciliation when material state findings require it
    -> Engineering Knowledge Base
    -> Engineering Compiler
    -> Orchestrator
    -> Isolated Execution Environment
    -> Verification Engine
    -> Validated Checkpoint
    -> Persistent Memory and Skills
```

The MVP will be deterministic in workflow and probabilistic only inside bounded reasoning steps. Execution is sequential. An executor proposes a change; it never promotes its own output.

The initial Intent Compiler uses the human intention plus project memory and policy already supplied to it; it does not inspect the repository itself. If the State Compiler later discovers an inherited rule or fact that materially changes interpretation, safety, architecture, or behavior, the Orchestrator must re-run the Intent Compiler and persist a reconciled Intent IR version before engineering compilation.

## Architecture at a glance

DevHarness separates three authority domains:

- **Control Plane** — intent, state, engineering policy, orchestration, verification, checkpoints, and memory.
- **Execution Plane** — disposable, least-privilege environments where implementation attempts run.
- **Provider Plane** — normalized adapters for external models and providers; transport, not architectural authority.

The initial system is planned as a CLI-first modular monolith using Python 3.14, Pydantic v2, SQLite, Git CLI and worktrees, an encapsulated subprocess runner, and pytest.

## Repository status

This repository currently contains only the approved project foundation:

- contribution, security, and agent operating rules;
- the initial architecture and component boundaries;
- foundational Architecture Decision Records (ADRs);
- the Phase 0/MVP specification and acceptance gates.

There is deliberately no application package, executable, dependency manifest, database schema, or harness implementation yet. Implementation must begin only under an explicitly authorized Phase 0 task and follow [AGENTS.md](AGENTS.md).

## Public repository data safety

This repository is intentionally **PUBLIC**. Do not publish credentials, secrets, personal data, private endpoints, database or memory dumps, internal infrastructure details, or other sensitive artifacts in code, prompts, logs, traces, fixtures, examples, documentation, commits, issues, or pull requests. Use synthetic or thoroughly sanitized material and review staged content and history before publication.

## Documentation

- [Architecture](docs/architecture/overview.md)
- [Phase 0 / MVP specification](docs/specifications/phase-0-mvp.md)
- [ADR index](docs/adr/README.md)
- [Contributing](CONTRIBUTING.md)
- [Security policy](SECURITY.md)

## License

DevHarness is licensed under the [Apache License 2.0](LICENSE). This license was selected for an extensible engineering infrastructure project because it permits broad use while providing explicit patent terms and preserving attribution and notices.

## Naming note

DevHarness is a descriptive project name. This project is independent and is not affiliated with other products or organizations using similar names.
