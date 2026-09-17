---
description: Enforce project rules and conventions to the letter with zero tolerance ($* or target task/file)
---

# /strict [task | file | on | off]

Enforces project rules and conventions to the letter with zero tolerance.

Arguments: `$*`

## Steps:
1. **Check Documentation Existence**:
   - Verify that `docs/skill/RULES.md` and `docs/skill/CONVENTIONS.md` exist.
   - If missing, warn: "⚠️ `docs/skill/` rules not found. Run `/create-docs` first." and halt execution.

2. **Load Rules & Conventions**:
   - Read `docs/skill/RULES.md` and `docs/skill/CONVENTIONS.md` completely.
   - Extract all applicable coding rules, architecture constraints, naming conventions, import orders, and forbidden patterns (`❌`).

3. **Determine Mode of Operation**:
   - **Task execution (`/strict <task description>`)**:
     1. Identify all applicable rules from `RULES.md` and `CONVENTIONS.md` before writing or modifying any code.
     2. Execute the task strictly adhering to every rule to the letter. No shortcuts, no `any`, no implicit types, no forbidden patterns, no convention deviations.
     3. Refuse any requested implementation detail that contradicts a documented rule; explain why and implement the rule-compliant alternative.
     4. Verify all modified/created files against the rules checklist before reporting completion.
     5. Report the result with a mandatory **Strict Compliance Checklist**.
   - **Audit mode (`/strict` or `/strict <file/folder>`)**:
     1. If no argument is provided, inspect current unstaged and staged changes (`git diff`). If a file or folder path is given, inspect the target code.
     2. Cross-reference every touched or targeted line of code against all rules in `docs/skill/RULES.md` and `docs/skill/CONVENTIONS.md`.
     3. Output a **Strict Compliance Audit Report**:
        ```markdown
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

## Strict Mode Guarantees:
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
