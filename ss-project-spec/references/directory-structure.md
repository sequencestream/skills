# Specs Directory Structure Specification

This document defines the canonical directory structure for project specifications.
The root directory is `specs/` at the project root.

## Top-Level Structure

```
specs/
├── overview.md                 # (required) Knowledge base overview & usage rules
├── project.md                  # (required) Project overview, vision, stakeholders
├── constitution.md             # (recommended) Global constraints, hard rules, immutable decisions
├── glossary.md                 # (required) Global business glossary
│
├── non-functional/             # (recommended for >1 dev) Non-functional requirements
│   ├── performance.md          #   Performance targets, benchmarks, rate limiting
│   ├── security.md             #   Auth, data masking, encryption, compliance
│   └── availability.md         #   SLA, redundancy, DR, retry, degradation
│
├── architecture/               # (required) Global architecture
│   ├── architecture.md               #   Architecture overview & conventions
│   ├── adr/                    #   Architecture Decision Records
│   │   ├── adr.md           #     ADR numbering & writing conventions
│   │   └── deprecated/        #     Deprecated/superseded ADRs
│   └── domain-dependency.md    #   (recommended) Domain dependency map & topology
│
├── api/                       # (recommended) API contracts
│   └── <group>/               #   Grouped by business group
│       └── <name>-api.yaml    #     OpenAPI 3.x contract
│
├── flows/                     # (recommended) Cross-domain business flows
│   ├── flows.md               #   Index of all flow files
│   └── flow-<scenario>.md     #   Per-scenario flows: each flow's ordered steps, branches/exceptions
│
├── domains/               # (required) DDD bounded contexts
│   └── <group>/                #   Business group (kebab-case, e.g., commerce)
│       ├── <group>-overview.md #   (required) Group overview & domain list
│       └── <domain>/       #   One domain per directory (kebab-case)
│           ├── <domain>-overview.md     #   (required) Domain overview, ownership, status
│           ├── <domain>-spec.md         #   (required) Domain spec — quarterly stable
│           ├── <domain>-design.md       #   (required) Technical design
│           ├── <domain>-models.md       #   (recommended) Domain-specific entity models
│           ├── <domain>-test-spec.md    #   (recommended) Test specification
│           ├── features/       #   (optional) Feature specs — iteration-level
│           │   └── <domain>-<feature>.md #     One file per feature (kebab-case)
│           └── artifacts/      #   (optional) Diagrams, prototypes, references
│
├── shared/                     # (recommended) Cross-domain conventions
│   ├── models/          #   Shared domain models
│   ├── error-codes/            #   Error codes, messages, response format
│   ├── api-conventions/        #   API style, headers, response structure
│   ├── data-conventions/       #   Naming, types, enums, dictionaries
│   └── dev-conventions/        #   Coding, branching, commit conventions
│
├── testing/                    # (recommended) Global testing strategy
├── assets/                     # (optional) Global diagrams, static resources
└── archived/                   # (recommended) Deprecated content
```

## Directory-Level Rules

### `domains/<group>/<domain>/` — Per-Domain Documents

Each domain directory follows a standard template:

| File | Required? | Audience | Content |
|------|-----------|----------|---------|
| `<group>-overview.md` | **Yes** | All | Group scope, domain index, shared context |
| `<domain>-overview.md` | **Yes** | All | Scope, owner, status, dependencies, domain index |
| `<domain>-spec.md` | **Yes** | PM, QA, Developers | **Domain spec.** Core entities, universal rules, invariants |
| `<domain>-design.md` | **Yes** | Developers, Architects | Data model, state machine, API design, tech choices |
| `<domain>-models.md` | Recommended | Developers, Architects | Entity models, attributes, relationships for this domain |
| `features/<domain>-<feature>.md` | Optional | PM, QA, Developers | **Feature spec.** Feature-specific flows, acceptance criteria, edge cases |
| `<domain>-test-spec.md` | Recommended | QA | Test scenarios, edge cases, data setup |
| `artifacts/` | Optional | All | Sequence diagrams, ERDs, prototypes, research notes |

### `architecture/adr/` — ADR Conventions

- File pattern: `NNNN-title-with-dashes.md`
- NNNN is zero-padded, sequential (0001, 0002...)
- ADRs are **never deleted**. Move deprecated/superseded ADRs to `architecture/adr/deprecated/` with a header note linking to the replacement.
- ADRs in "proposed" status should be resolved within a sprint.

### `domains/<group>/<domain>/features/` — Feature Specs

Feature specs sit below the domain-level `<domain>-spec.md` and capture iteration-level behavior.

| Aspect | Domain Spec (`<domain>-spec.md`) | Feature Spec (`features/<domain>-<feature>.md`) |
|--------|------------------------|----------------------------------|
| Change frequency | Quarterly-stable | Per iteration |
| Content | Core entities, invariants, universal rules | Feature-specific flows, acceptance criteria |
| Owner | Domain lead | Feature team |
| Lifecycle | Exists as long as domain exists | Created per feature, archived when removed |

Rules:
- Feature specs **must not duplicate** rules already in the domain spec. Reference them instead.
- When a feature behavior stabilizes and becomes a domain invariant, promote it to `<domain>-spec.md`.
- One file per feature, kebab-case. E.g., `order-cancel.md`, `coupon-stack.md`.

### `api/<group>/` — API Contracts

- One OpenAPI 3.x YAML file per API, organized by business group (same groups as `domains/`).
- File naming: `<name>-api.yaml`. Use the service/resource name, not the group name (e.g., `order-api.yaml` under `api/commerce/`).
- API specs should reference the relevant domain spec and ADRs in their description fields.
- Shared API conventions (headers, response envelope, auth) belong in `shared/api-conventions/`.

### `shared/` — Convention Priority

If a convention exists in `shared/` and a domain-level document defines the same convention, the shared definition takes precedence. Domain docs should reference (not duplicate) shared conventions.

## When to Simplify

For small projects (1-2 developers, monolith, early stage), the minimum viable structure is:

```
specs/
├── project.md
├── constitution.md
├── glossary.md
└── domains/
    └── <group>/
        ├── <group>-overview.md
        └── <domain>/
            ├── <domain>-spec.md
            ├── <domain>-design.md
            └── <domain>-models.md
```

All other directories (features/, non-functional/, adr/, shared/, etc.) can be added incrementally as the project grows.
