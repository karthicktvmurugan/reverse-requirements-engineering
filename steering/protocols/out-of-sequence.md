# Out-of-Sequence Artefact Requests

## When to Load
Load this file when the user explicitly asks for a specific artefact before its dependencies in the dependency chain have been produced.

---

## Out-of-Sequence Artefact Requests

If the user explicitly asks for a specific artefact before its dependencies are resolved:

1. **Identify missing dependencies** — Check which prerequisite artefacts have not been produced.
2. **Offer two options:**

```text
You've asked for artefact 10 (High-Level Process Model), but the following dependencies are not yet produced:
- 05 Terminology
- 06 Data Model
- 07 Business Rules
- 08 Integrations
- 09 Communications

I can either:
1. Produce artefacts 05-09 in sequence first, then produce 10 (recommended for completeness)
2. Produce a preliminary version of 10 now based on direct code inspection (faster, but may have gaps)

Option 2 note: The preliminary artefact will be marked as "Draft — Pending Dependencies" and will be refined once the missing artefacts are produced. It may be incomplete or contain inaccuracies that would be caught by the dependency artefacts.

Which would you prefer?
```

3. **If user chooses the accelerated path:**
   - Produce the artefact by inspecting code directly (using the inspection method and technology checklists).
   - Mark the artefact header with:
     ```markdown
     ## Status
     Draft — Pending Dependencies (artefacts 05, 06, 07, 08, 09 not yet produced)
     ```
   - Add a note in `.progress.json`:
     ```json
     "outOfSequence": {
       "10": {"producedEarly": true, "missingDeps": [5, 6, 7, 8, 9], "needsRefinement": true}
     }
     ```
   - When the missing dependencies are later produced, automatically flag: "Artefact 10 was produced early. Now that its dependencies exist, shall I refine it?"
   - Do NOT skip the missing artefacts from the workflow — they still need to be produced in sequence.

4. **If user chooses the sequential path:**
   - Continue producing artefacts in order until the requested one is reached.
