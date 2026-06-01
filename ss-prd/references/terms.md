### Glossary

**Path**: `<prd-dir>/terms.md`

```markdown
# Glossary

## Overview
Definitions of specialized business terminology used across the product to ensure consistent understanding among all stakeholders

## Terms

| Term | Alias | Definition | Related | Notes |
|------|-------|------------|---------|-------|
```

**Field descriptions:**
- **Term**: Canonical name of the term (use this exact form in all other documents)
- **Alias**: Abbreviations, English/Chinese equivalents, or alternative names that may appear in conversations or legacy materials
- **Definition**: Clear, single-sentence business meaning; avoid circular definitions
- **Related**: Other terms, models, dictionaries, or roles that this term references
- **Notes**: Usage scope, common misunderstandings, or context-specific nuances

**Principles:**
- Include only business/domain terms that need explicit definition (industry jargon, internal vocabulary, role-specific concepts); exclude common vocabulary
- Each term has exactly one canonical name — use it consistently across all documents (overview, architecture, models, procedures, pages)
- Definitions describe business meaning, not technical implementation
- Distinguish terms from dictionaries: a **term** defines a concept (e.g., "Order"); a **dictionary** defines the enumerated values of an attribute (e.g., order statuses)
- Distinguish terms from models: a **term** is a vocabulary entry with a definition; a **model** has attributes, states, and relationships. A term may reference a model when the concept is also modeled
- Add new terms when they first appear in any document; update definitions when business meaning changes rather than introducing synonyms
- Sort terms alphabetically or by business group for readability
