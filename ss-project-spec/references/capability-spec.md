# Domain Spec Guide

Path: `specs/domains/<group>/<domain>/spec.md`

The domain spec captures the **stable business behavior** of a domain. It changes at a quarterly cadence and serves as the source of truth for what the domain does.

## Content

- **Overview** — Business purpose, scope, and boundaries of this domain
- **Core entities** — Business entities the domain manages, with their key attributes and relationships
- **Business rules** — Invariant rules that always apply, with IDs for cross-referencing
- **States & transitions** — Key domain states and what triggers transitions between them
- **Domain events** — Events this domain emits and who consumes them
- **Interactions** — Dependencies and contracts with other capabilities
- **Data dictionary** — Capability-specific business terms and their definitions

## Principles

- **Business language only.** Describe what, not how. No SQL, no API methods, no implementation details.
- **One spec per bounded context.** If a domain's concerns are large enough to split, it should be two capabilities.
- **Rules are referenceable.** Assign stable IDs to business rules so feature specs and test cases can reference them without duplication.
- **Global terms live in glossary.** If a term is used across multiple capabilities, define it in `specs/glossary.md` and reference it here.
- **Domain spec does not reference feature specs.** The domain layer has no knowledge of feature-level details.
