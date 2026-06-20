# Domain Document Guide

Path: `doc/domains/<group>/<domain>/<domain>.md`

The domain document is the **canonical entry point** for a bounded context. It opens with a short identity/lifecycle metadata header, then captures the domain's **stable business behavior (WHAT)** — what the domain does, its rules and invariants. It changes at a quarterly cadence and is the source of truth for the domain's behavior. Technical implementation lives in `<domain>-design.md` (HOW); entity structure lives in `<domain>-models.md`.

## Metadata Header

Open the file with a short metadata block (5–10 lines):

- **Identity** — Domain name, business group, one-line description
- **Ownership** — Responsible team or person
- **Status** — active / deprecated / planned
- **Dependencies** — Other domains this one depends on (directional: list only what it depends on)
- **API exposure** — Whether this domain exposes external APIs (`exposes-api: true | false`)
- **ADR references** — Architecture decisions affecting this domain

## Business Behavior

- **Overview** — Business purpose, scope, and boundaries of this domain
- **Core entities** — The business entities this domain owns, named with their business meaning. Keep it conceptual — attributes, types, and relationships belong in `<domain>-models.md`.
- **Business rules** — Invariant rules that always apply, each with a stable ID for cross-referencing
- **States & transitions** — Key business states and the rules governing transitions. The technical state machine (triggers, guards, post-actions) lives in `<domain>-design.md`.
- **Domain events** — Events this domain emits and who consumes them
- **Interactions** — Dependencies and business contracts with other domains
- **Data dictionary** — Domain-specific business terms and their definitions

## Principles

- **Business language only.** Describe what, not how. No SQL, no API methods, no implementation details.
- **Stay at the altitude code can't reach.** Describe intent, invariants, and boundaries — not step-by-step logic. Before writing a rule, ask "could the reader get this from the code?" If yes, link to the code instead of restating it.
- **One document per bounded context.** If a domain's concerns are large enough to split, it should be two domains.
- **Entities stay conceptual here.** Name entities and their business meaning; structural detail (attributes, types, relationships) is owned by `<domain>-models.md`. Don't restate the schema.
- **Rules are referenceable.** Assign stable IDs to business rules so feature documents and test cases can reference them without duplication.
- **Global terms live in glossary.** If a term is used across multiple domains, define it in `doc/glossary.md` and reference it here.
- **Metadata stays short.** The header is a directory-level index, not a design document. Status drives discoverability — active domains appear in indexes; deprecated ones redirect readers to archived replacements.
- **The domain document does not reference feature documents.** The domain layer has no knowledge of feature-level details.
