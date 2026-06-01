---
name: ss-skill-improver
version: 1.0.0
description: "Audits and rewrites an Agent skill's SKILL.md. Scores the target across name, description, scope, clarity, examples, safety, and degrees of freedom, then lists improvements. Rewriting applies only after user approval. Triggers when the user asks to review, audit, critique, score, refine, refactor, or improve a SKILL.md or skill definition. Does not trigger when the user wants to create a brand-new skill from scratch — a skill-creation flow handles that case instead."
---

# Skill Improve

Audit and rewrite an existing skill's `SKILL.md` against the embedded rubric below (derived from Anthropic's published Skill authoring best practices). Default behavior audits the skill and lists improvements; rewriting applies only after user approval. The rubric is the source of truth — do not re-research at runtime.

## Inputs

Accept any of:

- A path to a `SKILL.md` (or a directory containing one).
- The name of an installed skill — resolve in this order: `./<.claude|.agent>/skills/<name>/SKILL.md`, `~/<.claude|.agent>/skills/<name>/SKILL.md`, `./skills/<name>/SKILL.md`.
- Raw skill text pasted in the conversation (write to `/tmp/skill-<name>.md` before editing).

If the input is ambiguous, ask once which skill to target. Do not guess.

## Workflow

Copy this checklist and check off as you go:

```
- [ ] 1. Read the target SKILL.md and list its directory.
- [ ] 2. Score against the rubric (verdict + reason per row).
- [ ] 3. Report in three buckets: A) strengths, B) must-fix, C) could-consider.
- [ ] 4. Wait for user approval (default stop — do not proceed without approval).
- [ ] 5. [conditional] Draft the revision — apply only bucket B.
- [ ] 6. [conditional] Apply with Edit (surgical) or Write (full rewrite).
- [ ] 7. [conditional] Re-read; verify YAML parses and references resolve.
```

### Step 1 — Read

`Read` the SKILL.md. `ls` the skill directory. Note every file path referenced in the body — flag any that don't exist.

### Step 2 — Score against the rubric

Fill in ✅ / ⚠️ / ❌ / ➖ + a one-sentence reason for each row. Use ➖ (N/A) when a row genuinely doesn't apply to this skill — e.g., Row #14 Safety on a pure read-only skill, Row #20 Common Pitfalls on a brand-new skill with no usage history yet, Row #15 References on a single-file skill with no linked docs. N/A rows never produce must-fix items; do not stretch a rule to fit when it doesn't.

| # | Dimension | Rule |
|---|---|---|
| 1 | **Name** | ≤64 chars; lowercase letters, digits, hyphens only; no "anthropic"/"claude"; gerund or action form preferred |
| 2 | **Description size** | Non-empty; ≤1024 chars; no XML tags |
| 3 | **Description voice** | Third person — never "I can…" or "You can…" |
| 4 | **Description specificity** | Names the activity AND concrete trigger phrases / file types / scenarios |
| 5 | **Skip clause** | Tells Agent when NOT to fire (so it doesn't fight adjacent skills) |
| 6 | **Single responsibility** | One job, not a kitchen-sink mega-skill |
| 7 | **Concise body** | ≤500 lines; nothing that explains what Agent already knows |
| 8 | **Consistent terminology** | One term per concept throughout (not "field/box/control" mixed) |
| 9 | **Concrete examples** | Real commands, inputs, expected outputs — not abstract prose |
| 10 | **Degrees of freedom** | Prescriptive for fragile ops, flexible for open-ended ones |
| 11 | **Workflow clarity** | Multi-step tasks use numbered steps or checklists; mandatory vs. optional labeled |
| 12 | **Feedback loops** | Quality-critical operations include validate → fix → repeat |
| 13 | **Failure modes** | States what to do when prerequisites are missing or a step fails |
| 14 | **Safety** | Destructive / irreversible actions gated by confirmation or `--dry-run` |
| 15 | **References one level deep** | Files linked from SKILL.md directly — no chains `a.md` → `b.md` → `c.md` |
| 16 | **Forward slashes** | All paths use `/`, never `\` |
| 17 | **No time-sensitive content** | No "before/after <date>" — legacy info moved to a collapsed block |
| 18 | **No option dumps** | Picks one default + escape hatch; doesn't list five libraries |
| 19 | **File hygiene** | All referenced files exist; YAML parses; required keys present |
| 20 | **Common Pitfalls** | Has a section capturing real mistakes Agent makes with this skill |
| 21 | **Expression economy** | Sentence-level prose carries no slack: no filler (just/very/really), no needless nominalization (make a decision→decide), instructions use active imperative, no big word where a small one fits (utilize→use), prepositional chains and throat-clearing trimmed. Token cost compounds per invocation. |

### Step 3 — Report in three buckets

Group findings into exactly these three buckets. Always emit all three sections, even if a bucket is empty (say "none" in that case).

**A. What the skill does well** — call out strengths so they survive the rewrite. Quote evidence; a strength without an excerpt is hearsay and easy to delete by accident in Step 4.

```markdown
- <strength> — Row #N
  Excerpt: > <quoted text from the original>
```

**B. Must-fix (apply directly)** — high-impact problems that violate the rubric and will be patched in Step 5. For each:

```markdown
**Issue:** <one-line summary>
**Excerpt:** > <quoted text from the original>
**Fix:** <specific rewrite or edit>
**Rubric:** Row #N — <rule name>
```

A finding belongs in B only if it clearly violates a rubric rule. Borderline / stylistic items go in C, not B.

**C. Could consider (no action)** — nice-to-haves with no clear payoff. Just list them; do not apply. One bullet each, no quoted excerpts, no fixes drafted.

```markdown
- <suggestion> — Row #N (optional)
```

#### Sanity check before finalizing the report

Before moving to Step 4, re-read the bucket-B list through three lenses. Do not generate three separate reports — this is a single mental pass to catch blind spots:

- **Strict lens**: Is anything missing? Would a pedantic reviewer flag a rubric row I marked ✅ or ➖?
- **Pragmatic lens**: Is anything in B over-prescriptive — a stylistic preference dressed up as a violation? Demote it to C.
- **Expert lens**: For this skill's domain, did I miss a subject-matter issue the rubric doesn't cover? Add it to C (not B — rubric-grounded items only go in B).

Move items between buckets as needed.

### Step 4 — Wait for user approval

Present the report (buckets A, B, C) to the user. This is the default stopping point — do not continue to drafting and rewriting until the user explicitly approves. The user may:

- **Approve** the must-fix items → proceed to Step 5.
- **Request changes** to the report → adjust and re-present.
- **Reject** (audit only / don't change anything) → stop here. Do not draft or apply.

If the user says "proceed", "go ahead", "looks good", or similar, proceed to Step 5.

### Step 5 — Draft the revision

Apply **only bucket B** to the draft. Preserve:

- Everything called out in bucket A — do not refactor strengths.
- The skill's stated purpose. Improving ≠ expanding scope.
- The original language — if the source is Chinese, write the revision in Chinese.
- The original tone where it works.

For skills with executable code, also enforce: scripts handle errors rather than punt, no unjustified magic numbers, dependencies listed and verified.

**Simplification pass (Row #21).** When a bucket-B item targets expression economy, trim slack at the sentence level only: cut filler words, swap big words for small (utilize→use, in order to→to), de-nominalize (provide validation→validate), prefer active imperative, drop prepositional chains and throat-clearing intros. Guardrail: simplification touches wording, never meaning. Do not drop constraints, threshold values, mandatory/optional labels, or failure-handling. Ask of each cut word, "does the instruction change if this is gone?" — if yes, keep it.

### Step 6 — Apply

Write the revised file only after user approval (from Step 4). Use `Edit` for surgical changes, `Write` only for full rewrites. Print one line:

```
Updated <path>: applied <n> must-fix items (rows: <comma-separated row numbers>). <m> optional items listed but not applied.
```

**Never auto-apply bucket C.** Items in C are signals, not directives — leave them to the user.

### Step 7 — Verify

Re-read the file. Confirm: YAML parses, `name` and `description` are present and within limits, all referenced paths exist. If verification fails, fix and re-write before reporting done.

## Example report (abridged)

**A. What the skill does well**
- Description uses third person and names concrete file types — Row #3, #4
- Workflow uses numbered steps with a copy-able checklist — Row #11

**B. Must-fix (apply directly)**

**Issue:** Conflates PDF extraction and DOCX editing in one skill.
**Excerpt:** > "Use this skill for PDF text extraction and DOCX form filling…"
**Fix:** Split into `extracting-pdfs` and `editing-docx`. Keep this file scoped to PDFs only; move DOCX content to a sibling skill.
**Rubric:** Row #6 — Single responsibility

**C. Could consider (no action)**
- Add a `<details>` block with the v1 API signature for historical context — Row #17 (optional)
- Rename to gerund form `extracting-pdfs` — Row #1 (optional; current action form is acceptable)

## Principles

- **Embedded rubric, not live search.** The rubric here is canonical — running web searches per invocation wastes tokens.
- **Surgical over sweeping.** If only 1–2 rows fail, output a diff, not a full rewrite.
- **Preserve user voice.** Match original language and tone.
- **Verify after editing.** Never report "done" without re-reading the file.

## Common pitfalls

- Auto-rewriting and silently dropping a section the user added on purpose — read the body carefully before deleting.
- Flagging "consistent terminology" on intentional aliases (e.g., a glossary line) — check context before downgrading.
- Translating a Chinese skill into English because rubric examples are English — language is preserved.
- Adding a "Common Pitfalls" section that's empty or speculative — only add real ones; otherwise leave it out.
- Treating gerund form as mandatory — action-oriented and noun-phrase names are also acceptable per Anthropic's docs.
- Inflating the description with feature lists — discovery hinges on trigger phrases, not completeness.
- Over-simplifying in the name of Row #21 — rewriting a precise instruction (a threshold, a mandatory/optional label, a failure path) into a smooth-but-vague sentence. Expression economy trims wording, never semantics.
