# Change Record Guide

Path: `changes/<YYYY>/<MM>/<DD>/<YYYY-MM-DD-NNN-desc>/requirement.md`

A change record captures a proposed modification to the project. It lives in the `changes/` directory (sibling of `specs/`) and serves as the traceability link between the original request and the resulting spec updates.

## Content

- **Background & objectives** — What problem or opportunity drives this change
- **User scenarios** — Feature-level flows using Given/When/Then to describe expected behavior
- **Scope boundaries** — Explicitly what is in scope and what is out of scope
- **Impact analysis** — Which domains, ADRs, shared conventions, and non-functional documents are affected
- **Non-functional requirements** — Data scale, performance, security, compatibility constraints
- **Metadata** — Author, date, status (proposed / approved / implemented / rejected), version bump

## Principles

- **Scope boundaries prevent scope creep.** "Out of scope" is as important as "in scope." Be explicit.
- **Impact drives action.** Every affected document listed in the impact analysis must be updated before the change is marked "implemented."
- **Close the loop.** Once the change is applied to `specs/`, update the status and record the version bump. A change is not done until the spec is updated.
