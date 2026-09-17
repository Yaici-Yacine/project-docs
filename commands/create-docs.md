---
description: Create the full docs/ tree (skill/ and wiki/) from scratch based on project analysis
---

# /create-docs

Creates the full `docs/` tree from scratch in the current project.

## Steps:
1. Check if `docs/` already exists. If yes, warn the user and ask before overwriting.
2. Read the project to understand: tech stack, main features, folder structure (look at package.json, README, src/ or main source directory structure).
3. Create the following files using project templates:
   - `docs/skill/RULES.md` — filled with inferred rules based on the project's tech stack
   - `docs/skill/CONVENTIONS.md` — filled with naming conventions observed in the codebase
   - `docs/wiki/INDEX.md` — listing all discovered features/modules
   - `docs/wiki/features/[name].md` — one file per major feature found, using the feature template
4. After creation, report: "✅ docs/ created with X feature pages. Run `/audit-docs` to verify completeness."

## Rules:
- Never create empty placeholder files — always fill with real content based on project analysis.
- Infer rules from actual code patterns (e.g., if the project uses TypeScript strict mode, document it).
- Feature pages must include: purpose, location in codebase, how it works, key files, dependencies.
