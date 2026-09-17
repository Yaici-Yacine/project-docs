---
description: Validate user stories against INVEST criteria and verify code/tests compliance ($* or [feature-name] or [us: ...] [ac: ...])
---

# /validate-us [feature-name | us: ... [ac: ...]]

Validates user stories for structural quality (INVEST criteria) and verifies code & test compliance.

Arguments: `$*`

## Purpose:
Ensure user stories are actionable, measurable, testable, and accurately reflected in the codebase and test suite without gaps or inconsistencies.

## Steps:

1. **Parse Arguments**:
   - **Case A: Specific feature (`/validate-us [feature-name]`)**:
     1. Locate `docs/wiki/features/[feature-name].md`. If missing, suggest `/add-feature`.
     2. Check if a User Story (`> 👤 **User Story:** ...`) is present.
     3. Check if Acceptance Criteria (`📋 **Acceptance Criteria:**`) are present.
     4. If missing: warn and offer to generate a compliant User Story & Acceptance Criteria based on the feature's `Purpose` and codebase entry points.
     5. If present: extract the Persona, Action, Benefit, and Criteria.
   - **Case B: Inline User Story (`/validate-us us: [text] [ac: ...]`)**:
     1. Extract the raw text following `us:` and optional `ac:`.
     2. Validate structure against standard patterns in French or English:
        - FR: `En tant que [persona], je veux [action], afin de [bénéfice]`
        - EN: `As a [persona], I want to [action], so that [benefit]`
   - **Case C: Global audit (`/validate-us` without arguments)**:
     1. Scan all feature files in `docs/wiki/features/*.md`.
     2. Cross-reference which features contain a documented User Story and check their implementation status.
     3. Output a project-wide User Story coverage summary.

2. **Structural Validation (INVEST Criteria)**:
   Evaluate the user story against the 6 INVEST principles:
   - **I - Independent**: Can this story be developed and delivered autonomously?
   - **N - Negotiable**: Does it focus on the user problem rather than locking in rigid implementation details?
   - **V - Valuable**: Is the user benefit explicit, measurable, and relevant to the persona?
   - **E - Estimable**: Is the scope well-bounded and technically feasible?
   - **S - Small**: Is it focused on a single coherent capability rather than an oversized epic?
   - **T - Testable**: Can clear, unambiguous acceptance criteria (Given / When / Then) be verified?

3. **Code & Test Verification (for documented features)**:
   - Search codebase (routes, services, UI components) to verify if the capability promised in the story is implemented.
   - Check if automated tests (unit, integration, or E2E) cover the user journey and acceptance criteria.
   - Flag any discrepancies between the User Story's promised value and the actual implementation.

4. **Generate Acceptance Criteria (Gherkin format)**:
   - If acceptance criteria are missing or weak, generate 2 to 4 concrete Gherkin scenarios:
     ```gherkin
     Scenario: [Scenario Title]
       Given [initial state or preconditions]
       When [action taken by the user]
       Then [expected result / benefit fulfilled]
     ```

5. **Output Report**:
   ```markdown
   ## 👤 User Story Validation Report: [Feature Name or Inline]

   ### 📝 User Story
   > 👤 **User Story:** [Story text]

   ### 🔍 INVEST Quality Assessment
   - **Persona:** [Identified user persona | ⚠️ Vague persona]
   - **Action:** [Target action/capability]
   - **Benefit:** [Explicit value | ⚠️ Missing or tautological benefit]
   - **INVEST Score:** ✅ Compliant | ⚠️ Needs refinement — [Feedback]

   ### 💻 Implementation & Test Coverage
   - **Code Status:** ✅ Implemented in `[key file]` | ⚠️ Partially implemented | ❌ Not found
   - **Test Status:** ✅ Tested in `[test file]` | ⚠️ Missing edge case tests | ❌ No tests found

   ### 📋 Acceptance Criteria (Gherkin)
   ```gherkin
   Scenario: [Happy path]
     Given ...
     When ...
     Then ...
   ```

   ### 💡 Recommendations
   - [Concrete recommendations to improve the story, add tests, or fix code]

   ### 📖 Résumé & Compréhension de la User Story (Plain-Language Summary)
   - **Persona / Utilisateur cible :** [Qui utilise cette fonctionnalité et son rôle concret]
   - **Besoin concret :** [Ce que l'utilisateur cherche concrètement à faire, formulé sans jargon technique]
   - **Valeur apportée :** [Pourquoi cette fonctionnalité est essentielle, quel problème réel elle résout et le bénéfice direct]
   - **En clair (Synthèse) :** [Explication vulgarisée en 1 à 2 phrases pour comprendre instantanément l'intention et l'impact de l'US]
   ```

6. **Optional Auto-Update**:
   - If invoked on a feature file and approved, update `docs/wiki/features/[feature-name].md` to add/refine the User Story and inject the verified Acceptance Criteria into the page.
