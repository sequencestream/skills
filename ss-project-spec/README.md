# project-spec

**project-spec** defines and maintains the project-level specification documents using a structured, DDD-aligned directory layout.

This skill enforces:
- **Separation of concerns**: spec.md (what), design.md (how), api/ (contracts)
- **Single source of truth**: `specs/` for current state, `archived/` for deprecated content
- **Consistency across domains**: every domain follows the same document structure

## Install

Requires [skilladmin](https://github.com/sequencestream/skilladmin).

```bash
skilladmin install https://github.com/sequencestream/skills/ss-project-spec
```


## Usage

Once installed, the skill activates automatically when you ask about specifications, specs, domains, ADRs, or related topics. For example:

- "Initialize the specs directory for this project"
- "Add a domain for order management"
- "Create an ADR for using PostgreSQL"
- "Update the non-functional requirements for performance"
