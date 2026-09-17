---
description: Read and summarize the current project documentation (rules, conventions, features)
---

# /read-docs

Reads and summarizes the current project documentation.

## Steps:
1. Check that `docs/` exists. If not, suggest running `/create-docs`.
2. Read all files in `docs/skill/` and `docs/wiki/`.
3. Output a structured summary:
   - **Project Rules** (from RULES.md and CONVENTIONS.md)
   - **Feature Index** (from INDEX.md)
   - **Feature Details** (brief summary of each feature page)
4. If documentation seems outdated or incomplete, flag it clearly.

## Output format:
```markdown
## 📚 Project Documentation Summary

### Rules & Conventions
[key rules extracted]

### Features ([N] documented)
- **[feature]** — [one line description] → `docs/wiki/features/[feature].md`

### ⚠️ Issues Found
[list any gaps, missing pages, outdated content]
```
