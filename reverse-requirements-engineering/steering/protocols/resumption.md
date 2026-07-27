# Resumption Protocol

## When to Load
Load this file when the skill is re-invoked and a `.progress.json` exists in an output folder, or when context has been compacted mid-session and the agent needs to recover its position in the workflow.

---

## Resumption Protocol

When the skill is re-invoked and a `.progress.json` exists:

1. Read `.progress.json` to determine workflow state.
2. Check `lastSessionDate` against current date.
3. If more than 24 hours have passed since last session:
   - Ask: "The last session was on {date}. Has the codebase changed since then?"
   - If YES: Ask which repos changed. Run a targeted re-inspection of changed areas. Compare against existing artefacts and flag impacts before continuing.
   - If NO: Resume from next artefact after `lastCompletedArtefact`.
4. If within 24 hours: Resume directly without asking.
5. **Check `cascadeLog` for pending or recent cascades** (see Cascade Verification below).
6. Load the steering file for the next artefact.
7. Read dependency artefacts listed in that steering file.
8. Continue production.

---

## Cascade Verification on Resume

After reading `.progress.json`, check if a `cascadeLog` array exists:

1. **If `cascadeLog` exists with entries where `status` = `"complete"`:**
   - Inform the user: "A cascade update was applied in the previous session covering: [list corrections]. All affected artefacts were updated."
   - No further action needed — proceed with next artefact.

2. **If `cascadeLog` exists with entries where `status` = `"pending"`:**
   - The cascade was NOT completed in the previous session.
   - Read the `corrections` list and `artefactsToUpdate` list from the pending entry.
   - Before producing any new artefact, apply the pending cascade updates to all listed artefacts.
   - After completion, update the entry's `status` to `"complete"` and add `completedDate`.
   - Only then proceed to the next artefact.

3. **If no `cascadeLog` exists:** Proceed normally.

### Cascade Log Format

```json
"cascadeLog": [
  {
    "date": "2026-07-22",
    "trigger": "User feedback on artefact 11 rework",
    "corrections": [
      "Description of correction 1",
      "Description of correction 2"
    ],
    "artefactsUpdated": ["07a", "08", "10a", "10d"],
    "status": "complete"
  }
]
```

---

## Mandatory Cascade Check After Any Rework or Feedback

When user feedback leads to corrections in ANY artefact:

1. **BEFORE marking the artefact as complete or proceeding to the next artefact**, the agent MUST:
   - Identify ALL terms, concepts, names, relationships, or facts that were changed.
   - Search across ALL previously generated artefacts for references to those changed items.
   - Apply corrections to every affected artefact.
   - Record the cascade in `.progress.json` under `cascadeLog`.

2. **The cascade check is BLOCKING** — do NOT offer to proceed to the next artefact until the cascade is verified complete.

3. **If context pressure prevents completing the cascade in the current session:**
   - Record the cascade entry with `status: "pending"` and include `artefactsToUpdate` listing which artefacts still need updating.
   - On resume, the resumption protocol will detect this and complete the cascade before producing new artefacts.

---

## Context Compaction Recovery

If context has been compacted mid-session (you lose track of where you are in the workflow):

1. Read `.progress.json` from the output folder to determine the current state.
2. The `lastCompletedArtefact` field tells you what was last finished.
3. Increment to the next artefact number.
4. Check `cascadeLog` for any pending cascades — complete them first.
5. Read the steering file for that next artefact.
6. Read the dependency artefacts listed in the steering file.
7. Continue production as normal.

Do not guess or rely on memory after compaction — always re-read `.progress.json` as the source of truth.
