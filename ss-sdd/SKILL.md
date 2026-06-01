---
name: ss-sdd
description: Lightweight AI Agent Harness / checkpoint-driven coding skill. For high-input, high-frequency, multi-turn coding and agentic tasks; the model is the primary driver of progress — decomposing, exploring, and advancing on its own — while the human keeps control through a minimal spec, restatement, checkpoints, approvals, evidence-based validation, and reverse sync.
---

# SDD

## Core Positioning

- The model has shifted from "assistant" to the primary driver of progress: it decomposes, explores, experiments, and advances; the human owns direction, boundaries, pacing, risk, and acceptance.
- The goal is not to reduce control, but to reduce low-value resident tokens.
- Control comes not from pre-specifying every step, but from setting gates at key moments: Restate, Checkpoint, Approval, Validation, Reverse Sync.
- The spec's first audience is the human (persisted task context and organizational memory); the model is only the second audience.
- For the model, the spec delivers four things: **attention focus** (keep the model on what currently matters), **information index** (look things up on demand instead of keeping everything resident), **defense against context rot** (use a written spec to fight forgetting and drift across long conversations), and **review aid** (provide a spec-vs-code cross-check baseline).

## Hard Constraints

- `Spec is Truth`: the spec is the persisted context, compressed memory, and source of truth for collaboration.
- `No Spec, No Code`: do not enter implementation before a minimal spec has been formed or updated.
- `No Approval, No Execute`: do not enter implementation or high-impact changes without explicit approval.
- `Restate First`: after the user gives a task, restate your understanding in your own words before moving to spec or plan.
- `Core Goal as Loop Anchor`: the current phase's core goal is the sole anchor of the loop; re-align against it before execution, after any deviation, and at validation time.
- `Checkpoint Before Execute`: before implementation, give a short checkpoint covering understanding, goal, next step, risks, and validation.
- `Done by Evidence`: completion must be proven by validation results and external feedback, not declared by the model.
- `Reverse Sync`: after execution, write the results, deviations, and validation conclusions back into the spec.
- `Resume Ready`: before long tasks or pauses, leave a minimal resume anchor in the spec to support restart and handoff.
- `Ask via Tool`: every approval, confirmation, choice, or pause-question must be raised through an interactive tool (`AskUserQuestion`, or a permission prompt) with concrete options, never as free text buried in the reply that the user has to answer in prose.

## Default Assumptions

- The model decomposes tasks on its own; do not force a long `Research / Innovate / Plan / Execute / Review` expansion.
- Don't split small tasks for the sake of protocol, and don't write long plans for big tasks just for ceremony.
- What must be preserved: the minimal spec, the upfront restatement, the pre-execution checkpoint, explicit approval, and the post-execution sync.
- Only re-read the currently relevant spec section; do not reload the whole file.

## Task Depth

### `zero` (No-Spec Channel)

- For purely mechanical changes (typo fixes, log additions, config value tweaks — single-point edits with no decision content).
- Skip the micro-spec, execute directly, and write back a one-sentence summary on completion.
- If complexity exceeds expectations, immediately upgrade to `fast` or `standard`.

### `fast`

- Write a micro spec first; no raw edits.
- Use 1–3 sentences to state the goal, files involved, main risks, and validation method.
- Execute only after explicit user approval.
- Upgrade to `standard` or `deep` when complexity rises.

### `standard`

- Default mode, suitable for most 2+ file changes, ordinary feature work, and bug fixes.
- Fill in the necessary context, maintain a lightweight spec, and persist it to disk.
- Give a short checkpoint before execution; after approval, implement and write back the conclusion.

### `deep`

- For ambiguous requirements, architectural changes, unknown root causes, cross-module/cross-project work, or long-chain iteration.
- Explicit analysis, option comparison, and risk discussion are allowed, but stay concise.
- Write the deep-thinking output back to the spec first, then send it for review; only enter implementation after approval.

## Minimal Workflow

- After the user gives a task, restate your own understanding to make sure the core goal is strongly aligned.
- Use a minimal spec to lock down goal, boundaries, facts, plan, and conclusions, and persist it as early as possible.
- In the spec, use 1–3 lines to state the `Done Contract`: what counts as done, what proves it, and what counts as not-yet-done.
- Before implementation, give a short checkpoint: `current understanding`, `core goal`, `next 1–3 actions`, `risks`, `validation method`.
- If tests, logs, or human feedback expose deviation, re-state — based on external evidence — "has the current core goal changed, and what is still missing" before deciding whether to continue or adjust.
- Execute only after explicit user approval requested via `AskUserQuestion`; if scope or approach changes, update the spec first and re-request approval the same way.
- After execution, write back `Change Log / Validation / Resume or Handoff`, and state "whether the current core goal has been proven done by evidence; if not, what is the next loop's core goal".

## When to Pause

In any of the following situations, pause first and explain why:

- A key ambiguity exists in requirements that would change implementation direction.
- A destructive, high-risk, or irreversible operation is needed.
- The change touches architecture, public interfaces, data models, or migration strategy.
- The change crosses projects without explicit user permission or with unclear scope.
- The existing spec is clearly wrong, outdated, or conflicts with the current code reality.
- No minimal spec has been formed yet, or no explicit execution approval has been given.

## Spec Path

1. `<root-session-dir>`: use the user-specified path, or default to `changes/`.
2. `<dev-session-dir>`:
   - If specified by the user, use it.
   - Else if requirements info is given, create `<root-session-dir>/<YYYY>/<MM>/<DD>/YYYY-MM-DD-<NNN>-<requirement-short-name>/` (`<NNN>` is a zero-padded daily sequence starting at 001).
   - Otherwise, ask the user for either a directory or requirements info via `AskUserQuestion`.
