---
description: Update docs/skill/RULES.md and CONVENTIONS.md by scanning codebase for discrepancies
---

# /update-rules

Updates `docs/skill/RULES.md` and/or `docs/skill/CONVENTIONS.md`.

## Steps:
1. Read current `docs/skill/RULES.md` and `docs/skill/CONVENTIONS.md`.
2. Ask: "What do you want to update? (rules / conventions / both)"
3. Show current content and ask what changes to make.
4. Apply the updates, preserving existing structure.
5. Add a `Last updated: [date]` line at the top.
6. Report what was changed.

If invoked without arguments, scan the codebase for patterns that contradict or are missing from the current rules, and suggest updates.
