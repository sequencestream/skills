# SDD(Spec-driven Development)

A lightweight, checkpoint-driven skill for high-input, multi-turn coding tasks.

The model drives progress — decomposing, exploring, and advancing on its own — while the human stays in control through a minimal spec, restatement, checkpoints, approvals, evidence-based validation, and reverse sync.

See [`SKILL.md`](./SKILL.md) for the full skill definition.

## Install

Installed via [skilladmin](https://github.com/sequencestream/skilladmin).

```bash
skilladmin install https://github.com/sequencestream/skills/ss-sdd
```

## Use

```
/ss-sdd implement OAuth refresh in the auth middleware
```

Claude Code will then follow the SDD loop:

1. **Restate** — confirms its understanding of the task.
2. **Spec** — writes a minimal spec to `changes/<YYYY>/<MM>/<DD>/...` (or a path you specify).
3. **Checkpoint** — surfaces goal, next steps, risks, and how it will validate.
4. **Approval** — waits for your go-ahead before editing code.
5. **Execute** — implements the change.
6. **Reverse Sync** — writes change log, validation results, and resume anchor back into the spec.

### Pick a depth

| Depth | When to use |
|-------|-------------|
| `zero` | Typo fixes, log lines, single-value config tweaks. No spec, just do it. |
| `fast` | 1–3 sentence micro-spec, then approval, then edit. |
| `standard` | Default. 2+ files, normal features and bug fixes, persisted spec. |
| `deep` | Ambiguous requirements, architecture changes, unknown root causes. |

You can hint the depth in your prompt (`/ss-sdd fast: rename FooBar → Foo`) or let the model pick.
