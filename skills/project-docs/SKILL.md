---
name: project-docs
description: Project documentation manager. Creates and maintains a docs/ tree with skill/ (project rules, conventions) and wiki/ (feature documentation). Use when running /create-docs, /read-docs, /strict, /add-feature, /validate-us, /sync-plan, /add-rule, /update-rules, /fix-feature, or /audit-docs commands.
---

# Project Docs Skill

## Purpose
Manage structured project documentation inside a `docs/` folder at the root of the project. This skill creates, reads, edits, and audits a two-part documentation system: **skill** (project rules & conventions) and **wiki** (feature documentation).

## Documentation Structure
When this skill is active, the project documentation lives at:
```
docs/
├── skill/
│   ├── RULES.md          ← Project coding rules, patterns, forbidden patterns
│   └── CONVENTIONS.md    ← Naming conventions, file structure, architecture decisions
└── wiki/
    ├── INDEX.md          ← Master index of all documented features
    └── features/
        └── [feature-name].md  ← One file per feature/module
```

## When to Use
Load this skill when:
- Setting up documentation for a new or existing project
- Adding documentation for a new feature
- Reading project rules before implementing something
- Updating project conventions
- Enforcing project rules and conventions strictly when coding, refactoring, or reviewing
- Validating user story quality and acceptance criteria with codebase tests
- Synchronizing implementation plans with completed code
- Auditing documentation completeness

---

## Available Slash Commands
Every command is available as its own individual skill and slash command:
- `/create-docs` — Initialize full `docs/` tree from scratch
- `/read-docs` — Read and summarize project rules, conventions, and features
- `/strict [task | file | on | off]` — Zero-tolerance enforcement of `docs/skill/` rules
- `/add-feature [name] [us: ...] [ac: ...]` — Document a feature with user story and acceptance criteria
- `/validate-us [name | us: ... [ac: ...]]` — Validate user story quality (INVEST) and verify code/tests
- `/sync-plan [name | all]` — Auto-sync implementation plan checkboxes with verified code delivery
- `/add-rule [rule]` — Add a rule to `RULES.md` or `CONVENTIONS.md`
- `/update-rules` — Scan codebase and update `RULES.md` / `CONVENTIONS.md`
- `/fix-feature [name]` — Update bug reports, missing details, or plan for a feature
- `/audit-docs` — Audit documentation completeness vs codebase reality
