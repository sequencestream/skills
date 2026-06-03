# specflow

A lightweight spec-driven development flow that runs as a single command.

The trunk — **requirement → spec → plan → develop** — runs inline in the main agent. `develop` is a complete delivery: it codes, tests, and syncs project-level docs in one step. On top of the trunk sits a single enhancement-judgement step that decides whether to dispatch optional review sub-agents (`spec-review`, `plan-review`, `develop-review`); each review feeds its opinions back to the matching trunk step, which decides on its own whether to improve before continuing.

See [SKILL.md](./SKILL.md) for the full step-by-step specification.

## Positioning

`ss-specflow` is a structured but low-ceremony development flow: a requirement → spec → plan → develop trunk with optional quality reviews. It enforces the boring-but-load-bearing steps — clarifying the requirement, structuring it, sketching a plan, then implementing with tests and doc sync — without heavy machinery, so the common case stays fast and readable.

## Why leaner

The trunk always runs inline. Reviews are dispatched as sub-agents only when a quick enhancement judgement says they're worth it, and they merely **feed opinions back** — each trunk step decides for itself whether to act, with no forced acceptance, no complexity matrix, and no fix-loop counters. Less to read, less to obey, same core discipline.

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
