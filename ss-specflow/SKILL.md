---
name: ss-specflow
description: A lightweight spec-driven development flow that takes one requirement through a four-step trunk — requirement → spec → plan → develop — run inline, where develop covers coding, testing, and project-doc sync as one complete delivery. On top of the trunk sits a single enhancement-judgement step that decides whether to enable optional review sub-agents (spec-review, plan-review, develop-review); each review feeds its opinions back to the matching trunk step, which decides on its own whether to improve before continuing. Trigger when the user supplies a requirement (or a session directory to resume) and wants a structured but low-ceremony flow — no complexity matrix, no fix-loop counters. 
argument-hint: [requirement description or session directory]
allowed-tools: Read, Write, Edit, Glob, Grep, Bash, Agent
---

Run a lightweight spec-driven flow. The **trunk** runs inline; a single **enhancement-judgement** step decides whether to add review sub-agents. Keep it simple — no complexity matrix, no fix-loop counters.

## Instructions

1. Resolve `<root-doc-dir>` (user-specified, else check `specs/`, then `doc/`) and `<root-session-dir>` (user-specified, else `changes/`).
2. Resolve `<session-dir>`:
   - If the user specifies one, use it.
   - Else if the user supplies a requirement, create `<root-session-dir>/<YYYY>/<MM>/<DD>/YYYY-MM-DD-<NNN>-<requirement-short-name>/` (`<NNN>` = zero-padded daily sequence from 001) and write the raw requirement to `<session-dir>/requirement.md`.
   - Else ask the user for one of the two.
3. Run the trunk in order: requirement → spec → plan → develop. After the `spec` and `plan` steps, hit the **enhancement judgement** to decide whether to dispatch the matching review sub-agent; after `develop`, optionally dispatch `develop-review`.
4. As each step (and any review) finishes, append one line to `<session-dir>/state.md` (see [State Tracking](#state-tracking)) so progress is recorded for resume.
5. On completion, output a summary noting which reviews ran and what changed.

## File Roles

| File | Produced by | Holds what |
| --- | --- | --- |
| `requirement.md` | requirement | The user's **raw requirement** verbatim (plus clarifying Q&A), unprocessed. |
| `spec.md` | spec | **Persisted source of truth**. Describes what, not how. See positioning below. |
| `plan.md` | plan | **The bridge from what to how** (technical solution). See positioning below. |
| `state.md` | every step | **Resume log**: one line per step (and review) with status + timestamp. See [State Tracking](#state-tracking). |

`spec.md` is the task's **persisted source of truth and compressed shared memory**. Its first audience is the **human** (durable task context and organizational memory); the model is the second audience, for which it provides **attention focus** (what currently matters), an on-demand **information index** (look things up instead of keeping everything resident), a **defense against context drift** across long sessions, and a **spec-vs-code review baseline**. It carries: background, intent, functional scenarios with acceptance criteria, constraints, scope, and a **Done Contract** — what counts as done, what proves it, and what counts as not-yet-done. Keep it current; reverse-sync conclusions and deviations back into it.

`plan.md` turns the spec's *what* into a deliberate *how*. Beyond the chosen approach, affected files/modules, and testing/doc-sync notes, it records the **trade-offs weighed and the options rejected**, so design decisions stay auditable and every change in develop (and every review finding) can trace back to a recorded choice rather than an ad-hoc one.

Implementation code, tests, and project-level docs (e.g. a project-level spec) are written by the develop step directly into the project's own locations — no separate artifact is kept in `<session-dir>`.

## Flow & Feedback

The four trunk steps run inline; enhancement steps all run as sub-agents, each feeding its opinions back to the matching trunk step, which decides on its own whether to adopt the improvements before continuing.

```
requirement -> spec --(judge?)--> plan --(judge?)--> develop --(judge?)--> done
                 ^                  ^                    ^
                 |                  |                    |
            [spec-review†]     [plan-review†]     [develop-review†]
            (feedback back)    (feedback back)    (feedback back)
```

`†` = runs as a sub-agent; `[brackets]` = enabled only when the enhancement judgement says it's needed.

**Enhancement judgement:** after each trunk step produces its artifact, judge whether that step warrants an independent review (lean toward enabling when it touches core/shared logic, data models or interfaces, non-trivial trade-offs, multi-file changes, or carries uncertainty; skip for trivial, mechanical, single-point changes). Keep the judgement quick — no assessment matrix; when in doubt, enable it.

**Feedback mechanism:** the review sub-agent writes its opinions and returns; the matching trunk step reads them and **decides for itself** what to adopt, whether to revise the corresponding file (spec.md / plan.md) or the code, then moves on. No forced acceptance, no fix-loop counting.

### Dispatching review sub-agents

Dispatch via the `Agent` tool with `subagent_type` set to `general-purpose`. The prompt must be self-contained: the role, the actual path(s) of the file(s) to review (substituted, not a placeholder), that step's core principles, and an instruction to "return opinions consumable by the matching step (format is your call); do not edit the trunk files directly." After it returns, the trunk step decides how to apply them.

## Core Principles per Step

**requirement** — capture the raw requirement faithfully.
- Append clarifying Q&A to `requirement.md`; on unclear points ask and offer options.
- **Don't assume, don't hide confusion, surface trade-offs** — present competing interpretations rather than picking silently.

**spec** — turn the raw requirement into a structured one.
- Define **strong, verifiable** acceptance criteria; reject vague goals like "make it work."
- Describe **what, not how** — no implementation detail.
- Load only docs relevant to the scope; note inconsistencies between existing docs for later.
- *spec-review (optional sub-agent)*: check completeness, consistency, testability, and ambiguity; feed opinions back to the spec step.

**plan** — design the technical solution for `spec.md`.
- **Simplicity first** — ask "would a senior engineer call this overcomplicated?"
- **Fit the existing world** — reuse established patterns over new abstractions.
- Avoid over-engineering and speculative extensibility; think deeply on trade-offs but keep the written plan tight.
- *plan-review (optional sub-agent)*: simpler approaches win, reject complexity that doesn't pay for itself; flag only material improvements; check correctness, design fit, and risks. Feed opinions back to the plan step.

**develop** — implement `plan.md` as a **complete delivery** (code + tests + docs).
- **Surgical changes** — touch only what the task needs; don't refactor what isn't broken or reflow formatting; match existing style; every changed line traces back to the plan.
- Clean up orphans **your** changes created; don't delete pre-existing dead code (mention it instead).
- **Test** — cover every acceptance criterion and its edge cases, one comment per test; make tests pass and run lint.
- **Sync docs** — update affected project-level docs in their existing format; touch shared/global docs only when genuinely impacted. No separate doc-sync step.
- *develop-review (optional sub-agent)*: review the diff against `plan.md` for correctness, design fit, security, concurrency, and error handling — not style nits; keep suggestions surgical, don't regress passing tests. Feed opinions back to the develop step.

## State Tracking

Keep `<session-dir>/state.md` as the resume log:
- one line per step (and per review), appended as it finishes, recording the step, its status, and a timestamp. 
- Mark a step `done` only after its artifact (or, for develop, the code/tests/docs) is in place. 
- Record skipped reviews too, with the reason, so the judgement trail is auditable.

## Resume Support

On start, if `<session-dir>` already holds partial artifacts, read `<session-dir>/state.md` to see which steps finished, cross-check against the existing `requirement.md` / `spec.md` / `plan.md`, and continue from the first unfinished trunk step; do not repeat completed steps. 
If `state.md` is missing, fall back to inferring progress from which artifacts exist.
