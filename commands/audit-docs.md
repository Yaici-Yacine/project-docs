---
description: Audit documentation completeness against the actual codebase
---

# /audit-docs

Audits documentation completeness against the actual codebase.

## Steps:
1. Read `docs/wiki/INDEX.md` to get the list of documented features.
2. Scan the project's main source folders to discover all features/modules.
3. Cross-reference: find features in code NOT documented, and docs for features that no longer exist.
4. Check each existing feature page for missing sections.
5. Output audit report:

```markdown
## 📋 Documentation Audit Report

### ✅ Well Documented ([N] features)
[list]

### ❌ Undocumented Features ([N] found in code)
[list with suggested file paths]

### 🗑️ Stale Docs ([N] pages for removed features)
[list]

### ⚠️ Incomplete Pages ([N] pages with missing sections)
[feature] — missing: [section names]

### Recommended Actions
1. Run `/add-feature [name]` for each undocumented feature
2. Delete stale pages: [list]
3. Update incomplete pages
```
