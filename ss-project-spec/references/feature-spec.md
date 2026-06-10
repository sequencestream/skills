# Feature Spec Guide

Path: `domains/<group>/<domain>/features/<domain>-<feature>.md`

A feature spec captures **iteration-level behavior** — a single feature's what and how in one file. Features are small enough that splitting spec and design would be overhead.

## Content

- **Overview** — What this feature does, what problem it solves, scope boundaries. State **non-goals** ("what this feature does NOT do") explicitly — omitting them lets implementations over-generate.
- **User scenarios** — Concrete flows using Given/When/Then. Cover five categories, not just the happy path: success, error, edge/boundary, idempotency (for any write), and **anti-scenarios**.
- **Business rules** — Feature-specific rules (not shared; shared rules belong in `<domain>-spec.md`)
- **Domain references** — Which entities, states, and domain rules from `<domain>-spec.md` this feature leverages
- **Technical notes** — Data changes, API changes, implementation decisions
- **Acceptance criteria** — Verifiable checks that define when this feature is done. Each must be coverable by an automated test.

## Principles

- **No duplication of domain rules.** If a rule already exists in `<domain>-spec.md`, reference it by ID. Do not restate it.
- **Promote when stable.** If a feature behavior becomes a domain invariant, move it to `<domain>-spec.md`.
- **One file per feature.** Keep it self-contained. If a feature needs multiple files, it may be too large.
- **Design overhead-free.** The file combines spec and design because the feature is small enough that separating them creates churn without clarity gain.

## Writing Given/When/Then

- **When = behavior, not implementation.** Write "user clicks Cancel," not "calls `cancelOrder()`." The scenario describes observable interaction, not the code path.
- **Then = observable outcome, not internal state.** Write the concrete state change, response, and **side effects** (logs, events, notifications) — not "the logic is correct."
- **Keep scenarios atomic.** If a scenario runs past ~10 lines, the granularity is too coarse; split it.
- **Anti-scenarios carry equal weight.** An anti-scenario states what must *never* happen (privilege escalation, double-refund, broken idempotency, amount/state corruption). Write at least one per core feature: it bounds what an implementation is allowed to do, the way the happy path describes what it must do.

## Acceptance checklist

- [ ] Each scenario has a clear Given / When / Then
- [ ] When describes user/system behavior, not implementation
- [ ] Then describes observable results and side effects, not internal state
- [ ] At least one error scenario and (for writes) one idempotency scenario
- [ ] At least one anti-scenario
- [ ] Non-functional expectations are quantified (no "fast," "high," "good")
- [ ] Non-goals are listed in the overview
