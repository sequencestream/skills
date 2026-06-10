# Constitution Guide

Path: `specs/constitution.md`

The constitution is the project's **highest-level constraint document** — the set of principles that almost never change. It is like a national constitution: it does not prescribe specific behavior, but defines the lines no spec, design, or line of code may cross. It sits above feature specs, above code, above personal preference.

## Content

- **Mission & values** — Project mission in one line, plus core values listed **in priority order** (e.g. user privacy > business growth, data correctness > performance). Include the rule for resolving conflicts between values.
- **Tech stack baseline** — Allowed core stack, technologies forbidden to introduce, and the exception process (e.g. RFC + architecture review) for adding new ones.
- **Security baseline** — Non-negotiable rules: PII encryption, data masking, TLS version, default-deny access control, no hardcoded secrets, audit logging for privileged operations.
- **Compliance baseline** — Applicable regulations (GDPR, CCPA, SOC2), data retention limits, user data export/deletion rights, breach notification windows.
- **Coding principles** — Repo organization, interface contracts, schema migration rules, testing floor, code review requirements.
- **AI engineering principles** — Rules for AI-generated code (must pass lint/test/security scan, no auto-merge), reversibility of operations, context/tenant isolation, least-privilege tool access.
- **Operations principles** — Release gating (canary stages), incident response SLAs, monitoring/SLO requirements.
- **Amendment procedure** — Who can propose changes, sign-off threshold, public-notice period, and how violations are handled.

## Principles

- **Stable.** Review annually. Frequent edits mean detail rules were mistaken for principles — move those to a domain spec or `shared/`.
- **Concise.** Aim for under ~3000 words. If it grows past that, the "handbook" leaked into the "constitution."
- **Executable, not aspirational.** Every clause must be answerable Yes/No to "was this violated?" Avoid slogans like "customer first" — they cannot be enforced.
- **Priority is explicit.** Values must be ordered, and the tie-break rule for conflicts must be written down.
- **Change-controlled.** Amendments require explicit stakeholder sign-off and a notice period. A constitution without named owners has no authority.
- **Aligned with reality.** It must match the current codebase. If a clause conflicts with what exists today, attach a remediation plan rather than pretending compliance.

## Relationship to other specs

The constitution sets the **absolute floor** (any spec that violates it is invalid). Domain/feature specs (`<domain>-spec.md`, `features/<domain>-<feature>.md`) provide capability- and feature-level detail beneath it. References flow downward only — specs cite the constitution, never the reverse. See `rules.md` § Spec Layering.
