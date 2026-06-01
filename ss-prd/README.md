# prd skill

A skill for product design (no code). Helps R&D teams produce structured PRDs covering roles, dictionaries, models, states, processes, applications, and pages, with built-in change traceability.

## Why

- **Separate WHAT from HOW**: PRDs describe product behavior, not implementation. This skill enforces that boundary so design stays durable as code evolves.
- **Structure beats prose**: Roles, dictionaries, models, states, processes, and pages are first-class documents — not buried in a wall of text — so each concern can be reviewed and updated independently.
- **Traceable changes**: Every change produces a change record, so you can answer "why does the product work this way?" months later.
- **Incremental, not versioned dumps**: Design documents are updated in place; history lives in change records. No stale `v2-final-FINAL.md` files.
- **LLM-friendly**: A consistent directory layout and document templates make it easy for both humans and AI agents to read, extend, and reason about the product design.

## Install

Installed via [skilladmin](https://github.com/sequencestream/skilladmin).

```bash
skilladmin install https://github.com/sequencestream/skills/ss-prd
```

## Usage

```
/ss-prd Design a coupon system: admins create coupons, users redeem at checkout

/ss-prd Add expiration date to coupons
```
