# ADR-0006: Keep knowledge, memory, skills, and checkpoints distinct

- Status: `APPROVED_DIRECTION_NOT_IMPLEMENTED`
- Date: 2026-08-14
- Owners: project maintainers
- Supersedes: none
- Superseded by: none

## Context

Unsourced or context-free remembered advice can become harmful. Technical guidance may expire with versions, while project facts and protected historical constraints follow different validity rules. Checkpoints answer a different question from durable knowledge.

## Decision

Represent important knowledge with type, provenance, scope, version or validity condition, confidence or evidence, and exceptions. Keep separate records for:

- user preferences;
- project-specific facts and decisions;
- reusable technical knowledge;
- skills;
- validated execution checkpoints.

Classify skills as technical-evolving, stable-engineering, project-specific, or protected-historical. Protected history is superseded with a new record, never silently overwritten. A successful observation begins as a candidate and becomes a validated skill only with sufficient repeatable evidence.

The MVP begins with structured metadata and textual retrieval. It does not require a vector database.

## Alternatives considered

- **Single undifferentiated memory store:** rejected because authority and validity would be ambiguous.
- **Periodic expiry of all knowledge:** rejected because historical and project constraints do not age like external APIs.
- **Vector-first RAG:** deferred until retrieval evidence shows it is necessary.

## Consequences

Knowledge capture requires more metadata and explicit lifecycle states. The system can explain why advice applies and preserve historical context without treating it as current policy.

## Verification

Tests must cover provenance retention, validity filtering, conflicting sources, protected-history supersession, project-rule precedence, and prevention of automatic skill promotion from a single observation.

## Revisit triggers

Revisit retrieval technology only after measuring failures of structured and textual selection. Do not relax provenance requirements.
