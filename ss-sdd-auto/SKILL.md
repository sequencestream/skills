---
name: ss-sdd-auto
description: Lightweight checkpoint-driven coding skill; the model decomposing, exploring, and advancing, while the human keeps control through a minimal spec, restatement, checkpoints, evidence-based validation, and reverse sync.
---

# SDD

## Core Positioning

- The agent decomposes, explores, experiments, and advances; the human owns direction, boundaries, pacing, risk, and acceptance.
- Control comes from setting gates at key moments: Restate, Checkpoint, Approval, Validation, Reverse Sync.
- The spec delivers: 
  - **attention focus** (keep on what currently matters).
  - **information index** (look things up on demand instead of keeping everything resident).
  - **defense against context rot** (fight forgetting and drift across long conversations).
  - **review aid** (provide a spec-vs-code cross-check baseline).

## Hard Constraints

- `Spec is Truth`: the spec is the persisted context, compressed memory, and source of truth for collaboration.
- `No Spec, No Code`: do not enter implementation before a minimal spec has been formed or updated.
- `Restate First`: after the user gives a task, restate your understanding in your own words before moving to spec or plan.
- `Core Goal as Loop Anchor`: the current phase's core goal is the sole anchor of the loop; re-align against it before execution, after any deviation, and at validation time.
- `Checkpoint Before Execute`: a short checkpoint before implementation, covering understanding, goal, next step, risks, and validation.
- `Done by Evidence`: completion must be proven by validation results and external feedback.
- `Reverse Sync`: write the results, deviations, and validation conclusions back into the spec.
- `Resume Ready`: before long tasks or pauses, leave a minimal resume anchor in the spec to support restart and handoff.
- `Ask via Tool`: every approval, confirmation, choice, or pause-question must be raised through an interactive tool (`AskUserQuestion`, or a permission prompt) with concrete options, never as free text buried in the reply that the user has to answer in prose.
- `Spec Self-Check`: check the spec across five dimensions — Completeness, Consistency, Testability, Unambiguity, Feasibility — and expose any gap rather than silently proceeding.

## Default Assumptions

- What must be preserved: the minimal spec, the upfront restatement, the pre-execution checkpoint, explicit approval, and the post-execution sync.
- Only re-read the currently relevant spec section; do not reload the whole file.

## Task Depth

- `zero` (No-Spec Channel): For purely mechanical changes (typo fixes, log additions, config value tweaks — single-point edits with no decision content). Skip the micro-spec, execute directly, and write back a one-sentence summary on completion.
- `fast`:  Write a micro spec first; no raw edits. Use 1–3 sentences to state the goal, files involved, main risks, and validation method.
- `standard`: Default mode, suitable for most 2+ file changes, ordinary feature work, and bug fixes. Fill in the necessary context, maintain a lightweight spec. Give a short checkpoint before execution. Implement and write back the conclusion.
- `deep` : For ambiguous requirements, architectural changes, unknown root causes, cross-module/cross-project work, or long-chain iteration. Explicit analysis, option comparison, and risk discussion are allowed, but stay concise. Write the deep-thinking output back to the spec first; For architecture-level or cross-module specs, you may delegate an independent verifier sub-agent to adversarially check the five `Spec Self-Check` dimensions; 

## When to Pause

In any of the following situations, pause first and explain why:

- A key ambiguity exists in requirements that would change implementation direction.
- A destructive, high-risk, or irreversible operation is needed.
- The change crosses projects without explicit user permission or with unclear scope.
- The existing spec is clearly wrong, outdated, or conflicts with the current code reality.

## Spec Path

1. `<root-session-dir>`: use the user-specified path, or default to `changes/`.
2. `<dev-session-dir>`:
   - If specified by the user, use it.
   - Else if requirements info is given, create `<root-session-dir>/<YYYY>/<MM>/<DD>/YYYY-MM-DD-<NNN>-<requirement-short-name>/` (`<NNN>` is a zero-padded daily sequence starting at 001).
   - Otherwise, ask the user for either a directory or intent request via `AskUserQuestion`.
