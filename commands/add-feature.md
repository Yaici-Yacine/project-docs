---
description: Add a new feature documentation page to docs/wiki/features/ (usage: /add-feature [feature-name] [us: ...] [ac: ...])
---

# /add-feature [feature-name] [us: ...] [ac: ...]

Adds a new feature documentation page to the wiki.

Arguments: `$*`

## Steps:
1. If no feature name provided, ask: "What feature do you want to document?"
2. Check if a user story was provided after `us:` — if yes, extract it as the US text. If not provided, skip (do not ask).
3. Check if acceptance criteria were provided after `ac:` — if yes, extract them as bullet criteria. If not provided, skip (do not ask).
4. Check if `docs/wiki/features/[feature-name].md` already exists. If yes, ask: "Update existing page or create new?"
5. Explore the codebase to find all files related to this feature.
6. Create `docs/wiki/features/[feature-name].md` using the structure below, filling it with real content.
7. Update `docs/wiki/INDEX.md` to add this feature to the index.
8. Report: "✅ Feature '[name]' documented at docs/wiki/features/[name].md"

## User Story & Acceptance Criteria Rule:
- If a `us:` argument was provided, write it as a blockquote at the very top of the feature page, BEFORE everything else:
```markdown
> 👤 **User Story:** As a [user], I want to [action], so that [benefit].
```
- If an `ac:` argument was provided alongside `us:`, format each criterion as an unchecked task item under the story:
```markdown
> 👤 **User Story:** As a [user], I want to [action], so that [benefit].
>
> 📋 **Acceptance Criteria:**
> - [ ] [Criterion 1 / Given... When... Then...]
> - [ ] [Criterion 2]
```
- If neither was given, omit this section entirely — do not add placeholders.

## Feature Page Sections (IN ORDER):
1. `> 👤 User Story` & `📋 Acceptance Criteria` — (only if provided via `us:` / `ac:`)
2. **Purpose** — What this feature does and why it exists
3. **Location** — Main files/folders (`src/features/auth/`, etc.)
4. **How It Works** — Step-by-step explanation of the logic
5. **Key Files** — Table of important files with their role
6. **Dependencies** — What this feature depends on (internal + external)
7. **Entry Points** — How other code uses this feature (main exports, API endpoints, etc.)
8. **Implementation Plan** — Step-by-step plan to build or extend this feature (see format below)
9. **Known Bugs & Issues** — Known bugs, edge cases, limitations (use `🐛` prefix per bug)
10. **TODOs** — Pending improvements or open questions

## Implementation Plan Format:
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
