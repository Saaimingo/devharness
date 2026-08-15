# Contributing to DevHarness

Thank you for helping build DevHarness carefully. The project is currently at the documentary foundation stage, before Phase 0 implementation.

## Before contributing

Read:

1. [AGENTS.md](AGENTS.md)
2. [Architecture overview](docs/architecture/overview.md)
3. [Phase 0 / MVP specification](docs/specifications/phase-0-mvp.md)
4. [ADR index](docs/adr/README.md)
5. [Security policy](SECURITY.md)

## Current contribution scope

Until Phase 0 implementation is explicitly opened, welcome contributions are limited to:

- correcting documentary inconsistencies;
- clarifying component boundaries and acceptance criteria;
- proposing ADRs;
- threat modeling and verification design;
- identifying missing assumptions, risks, or terminology.

Do not submit application code, dependencies, lockfiles, generated project scaffolding, provider SDK integrations, or CI automation during the foundation-only stage unless a maintainer explicitly opens that scope.

## Proposing a change

For a significant change:

1. describe the problem and evidence;
2. state what is in and out of scope;
3. identify affected invariants and ADRs;
4. compare viable alternatives and trade-offs;
5. define verification and rollback expectations;
6. add or update an ADR before implementation.

Use `docs/adr/0000-template.md` for a new decision record. A future direction that has not been implemented must use the status `APPROVED_DIRECTION_NOT_IMPLEMENTED`.

## Pull requests

Keep pull requests small and reviewable. Include:

- what changed and why;
- affected paths and boundaries;
- assumptions and unresolved questions;
- validation performed and exact results;
- risks and rollback plan;
- confirmation that no secrets or unrelated changes are included.

Passing tests are necessary once code exists, but they do not by themselves prove architectural correctness, scope compliance, or safety.

## Commit messages

Use short imperative subjects that describe one coherent change, for example:

```text
Document control and execution plane boundaries
Define Phase 0 verification gates
Clarify provider adapter authority
```

Do not force push shared branches. Do not rewrite published history unless a maintainer has explicitly authorized that exact operation and a safe coordination plan exists.

## License

By submitting a contribution, you agree that it will be licensed under the Apache License 2.0 and that you have the right to submit it.
