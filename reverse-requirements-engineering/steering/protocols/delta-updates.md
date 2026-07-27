# Delta Updates Protocol

## When to Load
Load this file when the user chooses to update existing artefacts with findings from a new or changed repository, or when presenting a delta update review gate.

---

## Delta Update Review Gate

When updating existing artefacts:

1. Present delta summary: new items, modified items (before/after), unchanged (count), superseded.
2. Wait for approval.
3. Apply changes with change log entry.
4. **Cascade check:** Review all other generated artefacts for consistency.

---

## Delta Comparison and Update Logic

When the user chooses to update existing artefacts with findings from a new repo:

1. **Read all existing artefacts** in the target folder.
2. **Analyse the new repo** using the standard Codebase Inspection Method.
3. **Categorise changes** into:
   - **Updated business requirements** — Existing requirements that are now better understood, expanded, or corrected.
   - **New features or capabilities** — Business behaviour that did not exist in the previous analysis.
   - **Previously missing integration details** — External integrations, contracts, or communication patterns now observable.
   - **Refined business rules** — Rules that are now more precisely understood or have new conditions/outcomes.
   - **New or updated entities** — Data model additions or changes.
   - **New or updated processes** — Process steps, actors, or flows not previously documented.
   - **New or updated scenarios and acceptance criteria** — Testable scenarios derived from the new evidence.
4. **Present a delta summary** to the user before making changes:
   - List what will be added.
   - List what will be modified (with before/after summary).
   - List what remains unchanged.
5. **Wait for user approval** before applying changes to artefact files.
6. **Apply changes** while preserving:
   - Existing traceability IDs (do not renumber).
   - Existing reviewed/approved content (do not overwrite without flagging).
   - Confidence levels (may increase with additional evidence).
7. **Cascade review** — After updating any artefact, review all previously generated artefacts for consistency. If upstream changes affect downstream artefacts, flag them and update accordingly.
