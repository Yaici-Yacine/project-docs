---
description: Create the full docs/ tree (skill/ and wiki/) from scratch based on project analysis
---

# /create-docs

Creates the full `docs/` tree from scratch in the current project.

## Steps:
1. Check if `docs/` already exists. If yes, warn the user and ask before overwriting.
2. Read the project to understand: tech stack, main features, folder structure (look at package.json, README, src/ or main source directory structure).
3. Create the following files using project templates (look in `${CLAUDE_SKILL_DIR}/templates/`, `./templates/`, or use the built-in reference below):
   - `docs/skill/RULES.md` — filled with inferred rules based on the project's tech stack
   - `docs/skill/CONVENTIONS.md` — filled with naming conventions observed in the codebase
   - `docs/wiki/INDEX.md` — listing all discovered features/modules
   - `docs/wiki/features/[name].md` — one file per major feature found, using the feature template
4. After creation, report: "✅ docs/ created with X feature pages. Run `/audit-docs` to verify completeness."

## Rules:
- Never create empty placeholder files — always fill with real content based on project analysis.
- Infer rules from actual code patterns (e.g., if the project uses TypeScript strict mode, document it).
- Feature pages must include: purpose, location in codebase, how it works, key files, dependencies.

## Built-in Template Reference:

### `docs/skill/RULES.md`
```markdown
# Project Rules
> Last updated: [YYYY-MM-DD]
> Project: [PROJECT_NAME]

## Tech Stack
- **Language**: [LANGUAGE]
- **Framework**: [FRAMEWORK]
- **Runtime**: [RUNTIME]

## Coding Rules
- [ ] Explicit types / strict typing
- [ ] Error handling in async operations

## Forbidden Patterns
- ❌ Hardcoded secrets / config
- ❌ Direct unauthorized state mutation
```

### `docs/skill/CONVENTIONS.md`
```markdown
# Project Conventions
> Last updated: [YYYY-MM-DD]

## Naming Conventions
- Files: kebab-case or camelCase matching codebase
- Components: PascalCase

## Directory Structure
- Follow existing patterns in src/
```

### `docs/wiki/INDEX.md`
```markdown
# Project Wiki Index
> Last updated: [YYYY-MM-DD]

## Features
- **[feature-name]** — Description → `docs/wiki/features/[feature-name].md`
```
