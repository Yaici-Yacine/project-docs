---
description: Update an existing feature page with bug reports, missing details, or plan updates
---

# /fix-feature [feature-name]

Updates an existing feature page with bug reports, missing details, or implementation plan updates.

Arguments: `$*`

## Steps:
1. If no feature name provided, ask: "Which feature do you want to fix/update?"
2. Check that `docs/wiki/features/[feature-name].md` exists. If not, suggest `/add-feature` instead.
3. Ask: "What do you want to fix? (bugs / details / implementation plan / all)"
4. Based on the answer:
   - **bugs** → Read the codebase for known issues, error handling gaps, edge cases. Add/update the `Known Bugs & Issues` section. Format each bug as: `🐛 **[Bug title]** — [description] — *Severity: low/medium/high*`
   - **details** → Re-scan the codebase for this feature and fill in any missing or outdated sections (How It Works, Key Files, Dependencies, Entry Points).
   - **implementation plan** → Update the `Implementation Plan` section: mark completed tasks `[x]`, add new tasks, update phase status.
   - **all** → Do all of the above.
5. Update `Last updated:` date in the file.
6. Report what was changed: "✅ Fixed [feature]: updated [sections changed]"

## Rules:
- Never delete existing bug entries — only add or update status.
- Mark resolved bugs with ~~strikethrough~~ and add `✅ Fixed in [version/date]`.
- If implementation plan doesn't exist yet, create it from scratch based on current codebase state.
