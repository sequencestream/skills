# Document Management Rules & Conventions

## Naming Conventions

| Element | Convention | Example |
|---------|-----------|---------|
| Domain groups | kebab-case | `commerce`, `platform`, `fulfillment` |
| Domain directories | kebab-case | `order-management`, `user-auth` |
| Group overview files | `<group>-overview.md` | `commerce-overview.md` |
| Feature document files | `<feature>.md` | `order-cancel.md` |
| ADR files | `NNNN-kebab-title.md` | `0001-use-postgres-for-primary-db.md` |
| YAML API contracts | `api/<group>/<name>-api.yaml` | `api/commerce/order-api.yaml` |
| Other markdown files | kebab-case | `domain-dependency.md` |

## Document Completeness Rules

1. **Every domain MUST have:**
   - `<domain>.md` — metadata header (owner, status, dependencies, scope) + business rules, behavior, invariants
   - `<domain>-design.md` — state machine, API design, tech choices
   - `<domain>-models.md` — entity definitions, attributes, relationships

2. **Every domain that exposes an API MUST have:**
   - A corresponding contract in `api/<group>/<name>-api.yaml` — OpenAPI 3.x

3. **If a domain has no public API, note it in the `<domain>.md` metadata header:**
   ```
   exposes-api: false
   notes: "Internal domain, no public interfaces"
   ```

4. **Every feature document SHOULD have:**
   - Feature overview and scope
   - Acceptance criteria (Given/When/Then)
   - Feature-specific business rules
   - Edge cases and error handling

5. **Every ADR MUST have:**
   - Status (proposed | accepted | deprecated | superseded)
   - Date
   - Context, Decision, Rationale, Consequences

## Document Layering

Documents are layered by change frequency. Each layer has a distinct purpose and constraints:

| Layer | File | Stability | Scope |
|-------|------|-----------|-------|
| Constitution | `constitution.md` | Immutable | Project-level hard rules, tech constraints, compliance |
| Domain document | `<domain>.md` | Quarterly-stable | Core entities, invariants, shared business rules |
| Feature document | `features/<domain>-<feature>.md` | Per iteration | Feature-specific flows, acceptance criteria, edge cases |

### Layering Rules

1. **Downward reference only.** Feature Documents can reference domain documents. Domain documents should not reference feature documents.
2. **No duplication.** If a rule exists in a higher layer, feature documents reference it (e.g., "per `<domain>.md` state machine"), not restate it.
3. **Promotion.** When a feature behavior stabilizes and becomes a domain invariant, promote it from feature document to domain document (`<domain>.md`).
4. **Constitution changes require review.** `constitution.md` modifications need explicit stakeholder sign-off.

## Consistency Rules

1. **No duplicate definitions.** If a term, model, or enum is used across multiple domains, define it in `shared/` and reference it. Do not copy-paste.

2. **Document and design must agree.** Every entity and state in `<domain>.md` must map to a corresponding element in `<domain>-design.md` / `<domain>-models.md`.

3. **ADR and design must agree.** Design decisions that are architecturally significant must reference an ADR.

4. **Global over local.** Shared conventions in `shared/` take precedence over domain-level conventions.

## Lifecycle Rules

1. **ADRs require human review before writing.** Never write an ADR file directly. Present the draft to the user, get explicit approval, then write it. New ADRs default to `Status: proposed` — only promote to `accepted` after the user confirms. This mirrors the stakeholder sign-off required for `constitution.md`.

2. **ADR statuses must be current.** "proposed" ADRs should resolve within a sprint. "superseded" ADRs must reference their replacement and move to `architecture/adr/deprecated/`.

3. **Archive, don't delete.** Deprecated domains go to `archived/`. ADRs are never deleted.

## Domain Metadata Header (in `<domain>.md`)

Each domain document opens with a lightweight metadata block documenting identity and lifecycle.

### Content

- **Identity** — Domain name, business group, one-line description
- **Ownership** — Responsible team or person
- **Status** — active / deprecated / planned
- **Dependencies** — Other domains this one depends on
- **API exposure** — Whether this domain exposes external APIs
- **ADR references** — Architecture decisions affecting this domain

### Principles

- **Keep it short.** The header is a directory-level index, not a design document. 5–10 lines is enough.
- **Status drives discoverability.** Active domains appear in indexes; deprecated ones redirect readers to archived replacements.
- **Dependencies are directional.** List only domains this one depends on, not everything that depends on it.

## Format Rules

- Use tables for listing structured information (attributes, states, transitions, roles).
- Use Mermaid for diagrams (state machines, sequence diagrams, architecture, flowcharts).
- Use YAML for API contracts (OpenAPI 3.x standard).
- Use kebab-case for directory and file names.
- Every directory should have a `README.md` or overview file explaining its purpose.
- Dates use `YYYY-MM-DD` format consistently.
- Business-semantic types over technical types (text, number, date, enum, reference — not varchar, int).

## Anti-Rationalization

| Excuse | Rebuttal |
|---|---|
| "This is a small project, the full structure is overkill" | Start with the minimal subset. But every domain still needs `<domain>.md` and `<domain>-design.md`. |
| "Glossary is obvious, we all know what this means" | If a term appears in more than one domain, add it. Onboarding new team members depends on this. |
| "ADRs slow us down" | A one-paragraph ADR is faster than explaining the same decision six times to six people. |
| "Documenting the exact logic here makes the document complete" | If the reader can recover it from the code, the code owns it. Restating logic only creates a second copy that drifts. Document captures intent/constraints code can't express. |
| "More implementation detail = more precise document" | Precision lives in the code; the document's job is the altitude code can't reach (why, boundaries, invariants). Sinking to detail trades a stable doc for a churning one. |
