---
name: ss-project-doc
description: >
  Define and maintain project-level documents under the doc/
  directory. Use this skill when the user asks to init/initialize documents, or create/update documents for
  domain, design, ADR, business flow, api, non-functional requirements, glossary terms, etc.
allowed-tools: Read, Write, Edit, Bash, Glob, Grep
---

# Project Doc Manager

You manage the project's documentation. Keep it consistent, complete, and properly structured. No code — you work exclusively with documentation.

## Core Principles

1. **Separate WHAT from HOW** — `doc.md` = business behavior. `design.md` = technical implementation. Never blur.
2. **Incremental in-place updates** — Update existing files rather than creating versioned filenames.
3. **Single source of truth** — `doc/` = current state. `archived/` = deprecated. Never mix.
4. **No code in document** — No source code, SQL schemas, or deployment configs. No code references.
5. **Consistency over creativity** — All domains follow the same template structure in `references/`.
6. **Specify the negative space** — A document must state non-goals ("what we won't do") and anti-scenarios ("what must never happen"), not only the happy path. Omitting boundaries lets implementations over-generate. Quantify every non-functional requirement — never "fast," "high," or "good."
7. **Code owns HOW, document owns WHY** — Code and document share the repo, so the document must not restate logic the code already expresses; duplicated detail only drifts. The document captures what code *cannot*: intent and rationale (why this exists, why this way), boundaries and constraints (what must never happen), cross-cutting decisions spanning many code locations, and stable business invariants. **Litmus test for every line:** "Could a reader recover this by reading the code?" If yes — delete it, link to the code. If no — it belongs here. When unsure, rise in altitude, don't sink.

> **Skip if** the user asks to write code, configure deployment, edit source files, or manage database migrations — this skill manages documentation only.

## Directory Overview

```
doc/
├── overview.md              # Knowledge base overview
├── project.md               # Project vision, stakeholders
├── constitution.md          # Global constraints, hard rules
├── glossary.md              # Business glossary
├── non-functional/          # performance, security, availability
├── architecture/adr/        # Architecture Decision Records
│   └── deprecated/          # Deprecated ADRs
├── flows/                   # Cross-domain business flows
│   ├── flows.md             #   Index of flow files
│   └── flow-<scenario>.md   #   Scenario flows & steps
├── domains/<group>/         # DDD bounded contexts
│   └── <domain>/
│       ├── <domain>.md          # Domain document (stable)
│       ├── <domain>-design.md        # Technical design
│       └── features/        # Feature Documents (iterative)
│           └── <domain>-<feature>.md
├── shared/                  # Cross-domain conventions
├── testing/                 # Testing strategy
├── assets/                  # Diagrams & resources
└── archived/                # Deprecated content
```

> **document layering**: `<domain>.md` = domain-level (quarterly stable). `features/<domain>-<feature>.md` = feature-level (iteration changes). See `references/feature-doc.md` for details.

See `references/doc-structure.md` for the full document with per-directory rules.

## Operations

Each trigger maps to a short procedure. Read the listed reference first, then create/update files, then run the Quality checks below.

| Trigger | Read | Create / update | Note |
|---|---|---|---|
| Init docs | `doc-structure.md` | `project.md`, `constitution.md`, `glossary.md`, minimal `domains/` | Start from the "When to Simplify" minimal set |
| Add a domain | `doc-structure.md`, `domain.md`, `domain-design.md` | `<group>/<domain>/` + `<domain>.md`, `<domain>-design.md`, `<domain>-models.md` | `<domain>.md`=WHAT, design=HOW, models=entities |
| Add a feature | `feature-doc.md` | `domains/<group>/<domain>/features/<domain>-<feature>.md` | Reference `<domain>.md`, never restate it |
| Add a flow | `doc-structure.md` | `flows/flow-<scenario>.md` + update `flows/flows.md` | Cross-domain flows & steps; reference domain rules by ID |
| Create an ADR | `adr.md` | `architecture/adr/NNNN-title.md` | Present draft for human review; write only after explicit approval. Zero-padded sequential number |
| Update NFR | `rules.md` | `non-functional/{performance,security,availability}.md` | Quantify every requirement |
| Add glossary terms | `rules.md` | `glossary.md` | Add if a term spans >1 domain |
| Archive content | `rules.md` | move to `archived/` (ADRs → `adr/deprecated/`) | Archive, never delete |
| Audit completeness | `rules.md` | report only | Check required files per domain |

## References

Read these files for detailed guidance on each aspect of document management:

| File | Purpose |
|------|---------|
| `references/doc-structure.md` | Complete directory layout, per-directory rules, variants |
| `references/rules.md` | Naming conventions, completeness rules, lifecycle, format rules, anti-rationalization |
| `references/constitution.md` | Guide for `doc/constitution.md` (project-level immutable constraints) |
| `references/domain.md` | Template for `domains/<group>/<domain>.md` (domain document) |
| `references/feature-doc.md` | Template for `domains/<group>/<domain>/features/<domain>-<feature>.md` |
| `references/domain-design.md` | Template for `domains/<group>/<domain>-design.md` |
| `references/adr.md` | Template for `architecture/adr/NNNN-title.md` |

## Quality

After creating or updating any document, verify:

1. All required sections from the template are present.
2. No code, SQL schemas, or implementation details leaked in.
3. Referenced files exist and paths are correct.

Fix any issues before reporting completion. If a template or reference file is missing, create the file from the corresponding template in `references/` before proceeding. If the user's request is unclear, ask for clarification rather than guessing the structure.
