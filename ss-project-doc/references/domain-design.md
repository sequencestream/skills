# Domain Design Guide

Path: `doc/domains/<group>/<domain>/<domain>-design.md`

The domain design describes the **technical implementation** of a domain. It translates the business behavior in `<domain>.md` into state machines, APIs, and technology choices.

## Content

- **Entity reference** — Point to `<domain>-models.md` for entity definitions, attributes, types, and relationships (the single source). Capture here only design-level concerns, not the catalog itself.
- **State machines** — States, transitions, triggers, preconditions, and post-actions
- **API design** — Endpoints, request/response shapes, auth requirements
- **Technology choices** — Key technology decisions with rationale
- **Non-functional considerations** — Performance targets, security approach, availability guarantees
- **Dependencies** — Internal and external dependencies with degradation behavior

## Principles

- **Behavior first, design second.** Every entity in the design must trace to a concept in `<domain>.md` (business meaning) and `<domain>-models.md` (structure). If it doesn't exist there, document it first.
- **Business-semantic types.** Use domain types (text, number, date, enum, reference) rather than database types (varchar, int). Leave physical types to the schema files.
- **State machines must be complete.** Every state must define its valid transitions. Implicit states (like "deleted" or "archived") must be explicit.
- **Architecturally significant decisions get an ADR.** If a design choice has lasting impact, trade-offs, or multiple valid options, record it in `architecture/adr/`.
- **Minimal tech detail.** Include enough for a developer to implement, but avoid framework-specific jargon. The design outlives any framework.
