# SequenceStream Skills

A collection of Agent skills for development.

All skills are installed via [skilladmin](https://github.com/sequencestream/skilladmin).

## Skills

| Skill | Intro | Install |
|-------|-------|---------|
| [ss-skill-improver](./ss-skill-improver) | Audits and rewrites a skill's `SKILL.md` against a best-practices rubric; rewriting applies only after user approval. | `skilladmin install https://github.com/sequencestream/skills/ss-skill-improver` |
| [ss-project-doc](./ss-project-doc) | Defines and maintains project-level documentation in a structured, DDD-aligned `specs/` layout (spec/design/api, ADRs, glossary). | `skilladmin install https://github.com/sequencestream/skills/ss-project-doc` |
| [ss-sdd](./ss-sdd) | Lightweight, checkpoint-driven skill for high-input, multi-turn coding — the model drives progress while the human keeps control via spec, checkpoints, and approvals. | `skilladmin install https://github.com/sequencestream/skills/ss-sdd` |
| [ss-prd](./ss-prd) | Product design skill — designs around system roles & responsibilities, dictionaries, models & attributes, states & transitions, processes & rules, applications, and page structures. No code involved. | `skilladmin install https://github.com/sequencestream/skills/ss-prd` |
| [ss-specflow](./ss-specflow) | Lightweight spec-driven flow — runs the requirement → spec → plan → develop trunk inline, with optional spec/plan/develop review sub-agents; no complexity matrix or fix-loop counters. | `skilladmin install https://github.com/sequencestream/skills/ss-specflow` |

See each skill's `README.md` and `SKILL.md` for full details.
