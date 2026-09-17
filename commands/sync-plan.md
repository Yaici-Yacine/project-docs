---
description: Scan codebase and automatically check off completed tasks in feature Implementation Plans ($* or [feature-name])
---

# /sync-plan [feature-name]

Scans the codebase and automatically synchronizes `Implementation Plan` checkboxes in feature documentation.

Arguments: `$*`

## Purpose:
Keep the `Implementation Plan` section (`- [ ] task` vs `- [x] task`) and phase status indicator (`Status: Not Started | In Progress | Complete`) of feature pages continuously aligned with actual code delivery.

## Steps:

1. **Locate Feature Documentation**:
   - If `feature-name` is provided, open `docs/wiki/features/[feature-name].md`. If missing, suggest `/add-feature`.
   - If not provided, ask: "Which feature plan do you want to synchronize? (or type 'all' to sync all feature plans)".
   - If `all` is chosen, sequentially sync every feature page in `docs/wiki/features/*.md`.

2. **Extract Implementation Tasks**:
   - Locate and parse the `## Implementation Plan` section.
   - If no implementation plan exists in the file, offer to generate one based on the current codebase state and roadmap.
   - Extract all phases and task items (`- [ ]` and `- [x]`).

3. **Verify Delivery in Codebase**:
   - For each task, inspect the project's source code, entry points, configuration, and tests:
     - Component / service / endpoint exists and is functional (not an empty stub or placeholder).
     - Exports and types match the described functionality.
     - Associated unit or integration tests are present.
   - If verified as implemented, update the checkbox to `- [x]`.
   - If already `- [x]` but code was removed or broken, flag it to the user.

4. **Update Status Indicator**:
   - Recalculate overall completion:
     - 0 tasks completed → `> 📌 Status: Not Started`
     - Some tasks completed → `> 📌 Status: In Progress`
     - All tasks completed → `> 📌 Status: Complete`

5. **Write Updates to File**:
   - Apply the updated checkboxes and status to `docs/wiki/features/[feature-name].md`.
   - Update `Last updated: [YYYY-MM-DD]` at the top of the file.

6. **Output Report**:
   ```markdown
   ## 🔄 Plan Sync Report: [feature-name]

   ### 📊 Progress: [X] / [Total] tasks completed ([Percentage]%)
   - **Overall Status:** Not Started | In Progress | Complete

   ### ✅ Newly Completed Tasks:
   - [x] Phase [N]: [task description] — Verified in `[source file]`

   ### ⏳ Remaining Pending Tasks:
   - [ ] Phase [N]: [task description] — Missing implementation
   ```
