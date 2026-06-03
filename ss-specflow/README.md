# specflow

A lightweight spec-driven development flow that runs as a single command.

The trunk — **requirement → spec → plan → develop** — runs inline in the main agent. `develop` is a complete delivery: it codes, tests, and syncs project-level docs in one step. On top of the trunk sits a single enhancement-judgement step that decides whether to dispatch optional review sub-agents (`spec-review`, `plan-review`, `develop-review`); each review feeds its opinions back to the matching trunk step, which decides on its own whether to improve before continuing.

See [SKILL.md](./SKILL.md) for the full step-by-step specification.

## Positioning

`ss-specflow` is a leaner parallel to [`ss-develop-pipeline`](../ss-develop-pipeline/). It keeps the same spirit — a structured requirement → design → implement flow with optional quality gates — but strips the heavy machinery so the common case stays fast and readable.

| | ss-develop-pipeline | ss-specflow |
| --- | --- | --- |
| Trunk | analyst → designer → developer | requirement → spec → plan → develop |
| Files | requirement-raw.md / requirement.md / design.md | requirement.md / spec.md / plan.md |
| Doc sync | separate `documenter` phase | folded into `develop` |
| Gates | complexity-assessment matrix, fix-loop counters (max 3), parallel orchestration | one simple enhancement judgement; review feedback, no counters |
| Reviews | improver / reviewer / tester / documenter sub-agents | spec-review / plan-review / develop-review sub-agents |

## Why leaner

Ad-hoc "vibe coding" skips the boring-but-load-bearing steps; `ss-develop-pipeline` enforces them but carries a lot of ceremony — a complexity matrix, fix-loop counting, mandatory sub-agent dispatch, hybrid/parallel orchestration. For many tasks that overhead outweighs the payoff.

`ss-specflow` keeps only the trunk and one enhancement-judgement step. The trunk always runs inline; reviews are dispatched as sub-agents only when a quick judgement says they're worth it, and they merely **feed opinions back** — each trunk step decides for itself whether to act, with no forced acceptance and no loop counting. Less to read, less to obey, same core discipline.

## Install

Installed via [skilladmin](https://github.com/sequencestream/skilladmin).

```bash
skilladmin install https://github.com/sequencestream/skills/ss-specflow
```

## Usage

```
/ss-specflow Add a /healthz endpoint that returns build SHA and uptime.

/ss-specflow changes/2026/06/03/2026-06-03-001-healthz-endpoint/
```
