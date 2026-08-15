# Architecture Decision Records

Architecture Decision Records (ADRs) capture decisions that materially constrain DevHarness. They explain context, choice, consequences, and implementation status.

## Status vocabulary

- `PROPOSED`
- `APPROVED_DIRECTION_NOT_IMPLEMENTED`
- `IMPLEMENTED`
- `SUPERSEDED_BY_ADR-NNNN`
- `REJECTED`

`APPROVED_DIRECTION_NOT_IMPLEMENTED` means the direction is accepted but no implementation claim is being made. Do not replace it with the ambiguous standalone word `APPROVED`.

## Records

| ADR | Decision | Status |
| --- | --- | --- |
| [0001](0001-modular-monolith-and-plane-separation.md) | Modular monolith with three authority planes | `APPROVED_DIRECTION_NOT_IMPLEMENTED` |
| [0002](0002-phase-0-technology-baseline.md) | Phase 0 technology baseline | `APPROVED_DIRECTION_NOT_IMPLEMENTED` |
| [0003](0003-sequential-deterministic-orchestration.md) | Sequential deterministic orchestration | `APPROVED_DIRECTION_NOT_IMPLEMENTED` |
| [0004](0004-git-worktree-sandbox-and-command-runner.md) | Git worktree sandbox and encapsulated command execution | `APPROVED_DIRECTION_NOT_IMPLEMENTED` |
| [0005](0005-evidence-based-state-promotion.md) | Evidence-based state promotion and checkpoints | `APPROVED_DIRECTION_NOT_IMPLEMENTED` |
| [0006](0006-provenance-aware-knowledge-and-memory.md) | Provenance-aware knowledge, memory, and skills | `APPROVED_DIRECTION_NOT_IMPLEMENTED` |
| [0007](0007-provider-neutral-core.md) | Provider-neutral core and adapter authority | `APPROVED_DIRECTION_NOT_IMPLEMENTED` |

Use [0000-template.md](0000-template.md) for new records.
