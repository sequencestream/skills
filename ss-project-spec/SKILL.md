---
name: ss-project-spec
description: >
  Define and maintain project-level specification documents under the specs/
  directory. Use this skill when the user asks to init/initialize specs, add a
  domain, document a spec or design, create an ADR, update non-functional
  requirements, add glossary terms, archive deprecated content, or audit spec
  completeness. Also triggers on keywords like "规范", "规格", "spec",
  "specification".
allowed-tools: Read, Write, Edit, Bash, Glob, Grep
---

# Project Spec Manager

You manage the project's specification documents. Keep them consistent, complete, and properly structured. No code — you work exclusively with documentation.

## Core Principles

1. **Separate WHAT from HOW** — `spec.md` = business behavior. `design.md` = technical implementation. Never blur.

2. **Incremental in-place updates** — Update existing files rather than creating versioned filenames.

3. **Single source of truth** — `specs/` = current state. `archived/` = deprecated. Never mix.

4. **No code in specs** — No source code, SQL schemas, or deployment configs.

5. **Consistency over creativity** — All domains follow the same template structure in `references/`.

6. **Specify the negative space** — A spec must state non-goals ("what we won't do") and anti-scenarios ("what must never happen"), not only the happy path. Omitting boundaries lets implementations over-generate. Quantify every non-functional requirement — never "fast," "high," or "good."

7. **Code owns HOW, spec owns WHY** — Code and specs share the repo, so the spec must not restate logic the code already expresses; duplicated detail only drifts. The spec captures what code *cannot*: intent and rationale (why this exists, why this way), boundaries and constraints (what must never happen), cross-cutting decisions spanning many code locations, and stable business invariants. **Litmus test for every line:** "Could a reader recover this by reading the code?" If yes — delete it, link to the code. If no — it belongs here. When unsure, rise in altitude, don't sink.

> **Skip if** the user asks to write code, configure deployment, edit source files, or manage database migrations — this skill manages documentation only.

## Directory Overview

```
specs/
├── overview.md              # Knowledge base overview
├── project.md               # Project vision, stakeholders
├── constitution.md          # Global constraints, hard rules
├── glossary.md              # Business glossary
├── non-functional/          # performance, security, availability
├── architecture/adr/        # Architecture Decision Records
│   └── deprecated/          # Deprecated ADRs
├── domains/<group>/         # DDD bounded contexts
│   └── <domain>/
│       ├── spec.md          # Domain spec (stable)
│       ├── design.md        # Technical design
│       └── features/        # Feature specs (iterative)
│           └── <feature>.md
├── shared/                  # Cross-domain conventions
├── testing/                 # Testing strategy
├── assets/                  # Diagrams & resources
└── archived/                # Deprecated content
```

> `changes/` is a sibling directory at project root level, alongside `specs/`. This skill only manages `specs/`.
> 
> **Spec layering**: `spec.md` = domain-level (quarterly stable). `features/<feature>.md` = feature-level (iteration changes). See `references/feature-spec.md` for details.

See `references/directory-structure.md` for the full specification with per-directory rules.

## References

Read these files for detailed guidance on each aspect of spec management:

| File | Purpose |
|------|---------|
| `references/directory-structure.md` | Complete directory layout, per-directory rules, variants |
| `references/rules.md` | Naming conventions, completeness rules, lifecycle, format rules, anti-rationalization |
| `references/constitution.md` | Guide for `specs/constitution.md` (project-level immutable constraints) |
| `references/capability-spec.md` | Template for `domains/<group>/<name>/spec.md` (domain spec) |
| `references/feature-spec.md` | Template for `domains/<group>/<name>/features/<feature>.md` |
| `references/capability-design.md` | Template for `domains/<group>/<name>/design.md` |
| `references/adr.md` | Template for `architecture/adr/NNNN-title.md` |
| `references/change-record.md` | Template for sibling `changes/` directory |

## Quality

After creating or updating any spec, verify:

1. All required sections from the template are present.
2. No code, SQL schemas, or implementation details leaked in.
3. Referenced files exist and paths are correct.

Fix any issues before reporting completion. If a template or reference file is missing, create the file from the corresponding template in `references/` before proceeding. If the user's request is unclear, ask for clarification rather than guessing the structure.
