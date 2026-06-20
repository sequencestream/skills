# ADR Guide

Path: `doc/architecture/adr/NNNN-title-with-dashes.md`

An Architecture Decision Record captures a significant architectural choice, its context, alternatives, and rationale. ADRs are **never deleted** — they form the permanent decision log of the project.

## Content

- **Status** — proposed / accepted / deprecated / superseded
- **Date** — When the decision was recorded
- **Context** — The issue, background, constraints, and current state that motivated this decision
- **Options considered** — Alternatives explored, with pros and cons for each
- **Decision** — The chosen approach and the rationale behind it
- **Consequences** — What becomes easier or harder as a result; migration notes if superseding a previous ADR
- **Compliance** — How the decision will be enforced (review guidelines, lint rules, automated checks)
- **References** — Related ADRs, spec documents, or external resources

## Principles

- **ADR is permanent.** Never delete an ADR. When a decision is no longer relevant, mark it as "deprecated" or "superseded", link to the replacement, and move it to `architecture/adr/deprecated/`.
- **One concern per ADR.** Each ADR covers one architectural decision. If two decisions are related but separable, write two ADRs.
- **Human review before writing.** Never write an ADR file without explicit user approval. Present the draft, wait for confirmation, then persist it with `Status: proposed`. Promote to `accepted` only after the user confirms the decision.
- **Record before implementation.** Write the ADR when the decision is made, not after implementation. The ADR guides the work, not the other way around.
- **Keep it scoped.** A good ADR is 2–5 paragraphs, not a design document. If you need more space, the decision may be too big and should be split.
- **Status is actionable.** "proposed" ADRs should resolve within a sprint. Stale proposals indicate stalled decisions.
