---
description: Add a new rule or convention directly to docs/skill/RULES.md or CONVENTIONS.md
---

# /add-rule [rule]

Adds a new rule or convention directly to `docs/skill/RULES.md` or `docs/skill/CONVENTIONS.md`.

Arguments: `$*`

## Steps:
1. If no rule text provided, ask: "What rule do you want to add? (describe it in plain language)"
2. Ask: "Is this a coding rule, architecture rule, or naming convention?"
   - Coding/architecture rule → goes into `docs/skill/RULES.md`
   - Naming/structure convention → goes into `docs/skill/CONVENTIONS.md`
3. Read the target file to find the right section to insert the rule.
4. Insert the rule in the appropriate section, formatted consistently with existing rules.
5. Update the `Last updated:` date at the top of the file.
6. Report: "✅ Rule added to docs/skill/[RULES|CONVENTIONS].md under section [section name]"

## Examples of Valid Rule Additions:
- `/add-rule no default exports` → adds to RULES.md under coding rules
- `/add-rule api route files must be named route.ts` → adds to CONVENTIONS.md under naming
- `/add-rule all zustand stores must be in src/stores/` → adds to CONVENTIONS.md under folder structure

If `docs/skill/` doesn't exist, suggest running `/create-docs` first.
