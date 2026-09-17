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

## Commands
All commands are also available as individual files under `commands/` (`commands/[command].md`) so that coding CLIs (Claude Code, OpenCode, Cursor, etc.) automatically detect and register them as slash commands.


### `/create-docs`
**Creates the full `docs/` tree from scratch in the current project.**

Steps:
1. Check if `docs/` already exists. If yes, warn the user and ask before overwriting.
2. Read the project to understand: tech stack, main features, folder structure (look at package.json, README, src/ structure).
3. Create the following files using the templates from `~/.config/opencode/skills/project-docs/templates/`:
   - `docs/skill/RULES.md` — filled with inferred rules based on the project's tech stack
   - `docs/skill/CONVENTIONS.md` — filled with naming conventions observed in the codebase
   - `docs/wiki/INDEX.md` — listing all discovered features/modules
   - `docs/wiki/features/[name].md` — one file per major feature found, using the feature template
4. After creation, report: "✅ docs/ created with X feature pages. Run `/audit-docs` to verify completeness."

Rules:
- Never create empty placeholder files — always fill with real content based on project analysis
- Infer rules from actual code patterns (e.g., if the project uses TypeScript strict mode, document it)
- Feature pages must include: purpose, location in codebase, how it works, key files, dependencies

---

### `/read-docs`
**Reads and summarizes the current project documentation.**

Steps:
1. Check that `docs/` exists. If not, suggest running `/create-docs`.
2. Read all files in `docs/skill/` and `docs/wiki/`.
3. Output a structured summary:
   - **Project Rules** (from RULES.md and CONVENTIONS.md)
   - **Feature Index** (from INDEX.md)
   - **Feature Details** (brief summary of each feature page)
4. If documentation seems outdated or incomplete, flag it clearly.

Output format:
```
## 📚 Project Documentation Summary

### Rules & Conventions
[key rules extracted]

### Features ([N] documented)
- **[feature]** — [one line description] → `docs/wiki/features/[feature].md`

### ⚠️ Issues Found
[list any gaps, missing pages, outdated content]
```

---

### `/strict [task | file | on | off]`
**Enforces project rules and conventions to the letter with zero tolerance.**

Steps:
1. Check that `docs/skill/RULES.md` and `docs/skill/CONVENTIONS.md` exist. If missing, warn: "⚠️ `docs/skill/` rules not found. Run `/create-docs` first."
2. Read both files thoroughly to load all project rules, coding standards, architecture constraints, naming conventions, import orders, and forbidden patterns (`❌`).
3. Determine execution mode based on arguments:
   - **Task execution (`/strict [task description]`)**:
     1. Identify all applicable rules from `RULES.md` and `CONVENTIONS.md` before writing or modifying any code.
     2. Execute the task strictly adhering to every rule to the letter. No shortcuts, no `any`, no implicit types, no forbidden patterns, no convention deviations.
     3. Refuse any requested implementation detail that contradicts a documented rule; explain why and implement the rule-compliant alternative.
     4. Verify all modified/created files against the rules checklist before reporting completion.
     5. Report the result with a mandatory **Strict Compliance Checklist**.
   - **Audit mode (`/strict` or `/strict [file/folder]`)**:
     1. If no argument is provided, inspect current unstaged and staged changes (`git diff`). If a file or folder path is given, inspect the target code.
     2. Cross-reference every touched or targeted line of code against all rules in `docs/skill/RULES.md` and `docs/skill/CONVENTIONS.md`.
     3. Output a **Strict Compliance Audit Report**:
        ```
        ## 🛡️ Strict Compliance Audit Report

        ### ✅ Verified Rules
        - [Rule/Convention]: [evidence of compliance]

        ### ❌ Rule Violations (Zero Tolerance)
        - **[file:line]** — `[Rule name]` — Violation: [description]. Required fix: [exact remedy].

        ### ⚠️ Warnings / Fragile Patterns
        - [Potential edge cases or borderline patterns]
        ```
     4. If violations exist, provide immediate fixes conforming strictly to the rules.
   - **Session toggle (`/strict on` | `/strict off`)**:
     - `on`: Activate persistent strict compliance mode for the entire session. All subsequent tasks must systematically verify and follow `RULES.md` and `CONVENTIONS.md` to the letter.
     - `off`: Deactivate persistent strict mode (revert to normal guidance).

Strict Mode Guarantees:
- **Zero Tolerance:** No exceptions, waivers, or compromises. Every documented rule must be respected to the letter.
- **Forbidden Patterns Are Blockers:** Any pattern marked `❌` in `RULES.md` is strictly prohibited.
- **Rule Citation:** Every compliance check or violation report must explicitly cite the rule from `RULES.md` or `CONVENTIONS.md`.
- **Compliance Checklist:** Every task executed under `/strict` must conclude with:
```markdown
### 🛡️ Strict Compliance Checklist
- [x] [Rule 1]: [how verified]
- [x] [Convention 1]: [how verified]
- [x] Zero forbidden patterns introduced
```

---

### `/add-feature [feature-name] [us: ...] [ac: ...]`
**Adds a new feature documentation page to the wiki.**

Steps:
1. If no feature name provided, ask: "What feature do you want to document?"
2. Check if a user story was provided after `us:` — if yes, extract it as the US text.
3. Check if acceptance criteria were provided after `ac:` — if yes, extract them as bullet criteria.
4. Check if `docs/wiki/features/[feature-name].md` already exists. If yes, ask: "Update existing page or create new?"
5. Explore the codebase to find all files related to this feature.
6. Create `docs/wiki/features/[feature-name].md` using the structure below, filling it with real content.
7. Update `docs/wiki/INDEX.md` to add this feature to the index.
8. Report: "✅ Feature '[name]' documented at docs/wiki/features/[name].md"

**User Story & Acceptance Criteria rule:** If a `us:` argument was provided, format it at the top of the feature page:
```markdown
> 👤 **User Story:** As a [user], I want to [action], so that [benefit].
>
> 📋 **Acceptance Criteria:**
> - [ ] Given [precondition], when [action], then [outcome]
```
If no `us:` was given, omit this section entirely — do not add a placeholder.

The feature page must contain these sections IN ORDER:
1. `> 👤 User Story` — (only if provided via `us:`)
2. **Purpose** — What this feature does and why it exists
3. **Location** — Main files/folders (`src/features/auth/`, etc.)
4. **How It Works** — Step-by-step explanation of the logic
5. **Key Files** — Table of important files with their role
6. **Dependencies** — What this feature depends on (internal + external)
7. **Entry Points** — How other code uses this feature (main exports, API endpoints, etc.)
8. **Implementation Plan** — Step-by-step plan to build or extend this feature (see format below)
9. **Known Bugs & Issues** — Known bugs, edge cases, limitations (use `🐛` prefix per bug)
10. **TODOs** — Pending improvements or open questions

**Implementation Plan format:**
```markdown
## Implementation Plan

### Phase 1: [Phase Name]
- [ ] 1.1 [concrete task]
- [ ] 1.2 [concrete task]

### Phase 2: [Phase Name]
- [ ] 2.1 [concrete task]
- [ ] 2.2 [concrete task]

> 📌 Status: Not Started | In Progress | Complete
```
The implementation plan must be filled with real tasks inferred from the codebase — not generic placeholders.


### `/validate-us [feature-name | us: ... [ac: ...]]`
**Validates user stories against INVEST criteria and checks code & test compliance.**

Steps:
1. Determine validation target:
   - **Feature name provided (`/validate-us [feature-name]`)**: read `docs/wiki/features/[feature-name].md`. Extract the user story and acceptance criteria. If absent, propose drafting one from the feature purpose and code.
   - **Inline user story provided (`/validate-us us: [text] [ac: ...]`)**: validate the story and criteria provided in FR or EN.
   - **No argument (`/validate-us`)**: scan all feature pages in `docs/wiki/features/` and produce a global user story coverage report.
2. Check structural quality using the **INVEST** framework:
   - Independent, Negotiable, Valuable (clear persona and benefit), Estimable, Small, Testable.
3. Verify implementation in code:
   - Confirm that routes, UI components, and services fulfill the user action and delivered benefit.
   - Check automated test presence and coverage for the story's happy path and edge cases.
4. Generate 2-4 concrete **Gherkin Acceptance Criteria** (`Given / When / Then`).
5. Output the validation report (INVEST score, code status, test status, Gherkin criteria, recommendations, and a final plain-language summary to explain and understand the User Story clearly).
6. If validated on an existing feature page, offer to update the feature file with the refined story and acceptance criteria.

---

### `/sync-plan [feature-name]`
**Scans codebase and automatically checks off completed tasks in feature Implementation Plans.**

Steps:
1. Locate the feature page in `docs/wiki/features/[feature-name].md` (or run on `all`).
2. Parse the `## Implementation Plan` section.
3. For each unchecked task (`- [ ]`), inspect the project source code, routes, and tests to verify if the functionality is implemented.
4. If verified, update the checkbox to `- [x]` in the file.
5. Update the phase and overall status (`Status: Not Started | In Progress | Complete`).
6. Update the `Last updated:` timestamp and output a synchronization report showing progress percentage and newly completed tasks.

---
---

### `/update-rules`
**Updates `docs/skill/RULES.md` and/or `docs/skill/CONVENTIONS.md`.**

Steps:
1. Read current `docs/skill/RULES.md` and `docs/skill/CONVENTIONS.md`.
2. Ask: "What do you want to update? (rules / conventions / both)"
3. Show current content and ask what changes to make.
4. Apply the updates, preserving existing structure.
5. Add a `Last updated: [date]` line at the top.
6. Report what was changed.

If invoked without arguments, scan the codebase for patterns that contradict or are missing from the current rules, and suggest updates.

---

### `/add-rule [rule]`
**Adds a new rule or convention directly to `docs/skill/RULES.md` or `docs/skill/CONVENTIONS.md`.**

Steps:
1. If no rule text provided, ask: "What rule do you want to add? (describe it in plain language)"
2. Ask: "Is this a coding rule, architecture rule, or naming convention?"
   - Coding/architecture rule → goes into `docs/skill/RULES.md`
   - Naming/structure convention → goes into `docs/skill/CONVENTIONS.md`
3. Read the target file to find the right section to insert the rule.
4. Insert the rule in the appropriate section, formatted consistently with existing rules.
5. Update the `Last updated:` date at the top of the file.
6. Report: "✅ Rule added to docs/skill/[RULES|CONVENTIONS].md under section [section name]"

Examples of valid rule additions:
- `/add-rule no default exports` → adds to RULES.md under coding rules
- `/add-rule api route files must be named route.ts` → adds to CONVENTIONS.md under naming
- `/add-rule all zustand stores must be in src/stores/` → adds to CONVENTIONS.md under folder structure

If `docs/skill/` doesn't exist, suggest running `/create-docs` first.

---

### `/fix-feature [feature-name]`
**Updates an existing feature page with bug reports, missing details, or implementation plan updates.**

Steps:
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

Rules:
- Never delete existing bug entries — only add or update status
- Mark resolved bugs with ~~strikethrough~~ and add `✅ Fixed in [version/date]`
- If implementation plan doesn't exist yet, create it from scratch based on current codebase state

---

### `/audit-docs`
**Audits documentation completeness against the actual codebase.**

Steps:
1. Read `docs/wiki/INDEX.md` to get the list of documented features.
2. Scan the project's main source folders to discover all features/modules.
3. Cross-reference: find features in code NOT documented, and docs for features that no longer exist.
4. Check each existing feature page for missing sections.
5. Output audit report:

```
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

---

## Rules
- Always read the project before writing documentation — never invent content
- Keep documentation in sync: when adding a feature page, always update INDEX.md
- Dates: use ISO format (YYYY-MM-DD)
- Rules and conventions may include links to official documentation when those links clarify standards, APIs, or framework behavior
- Feature file names: lowercase-kebab-case (e.g., `user-authentication.md`)
- Never overwrite existing docs without user confirmation
- When unsure about a feature's purpose, look at tests and usage sites in the codebase
- User stories (`us:`) are optional — never ask for them if not provided, never add placeholder text
- Under `/strict`, rules in `docs/skill/` are hard constraints: zero tolerance for forbidden patterns, missing types, or naming deviations
- User stories validated with `/validate-us` must follow INVEST criteria and specify verifiable acceptance criteria (Gherkin)
- Implementation plans synchronized with `/sync-plan` must reflect actual working code, not placeholders

## Anti-Patterns
- ❌ Creating empty/placeholder documentation
- ❌ Copy-pasting code into docs — explain behavior, don't paste implementation
- ❌ Documenting implementation details that change often — focus on stable interfaces
- ❌ Forgetting to update INDEX.md when adding/removing feature pages
