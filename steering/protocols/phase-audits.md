# Phase Boundary Audits

## When to Load
Load this file when the agent reaches a phase boundary (after artefacts 05, 09, 12, or 16 are approved) and needs to perform a completeness self-audit before proceeding to the next phase.

---

## Completeness Self-Audit (Phase Boundary Audits)

The agent performs incremental completeness checks at natural phase boundaries throughout the workflow — not just at the end. This ensures quality regardless of where the user decides to stop.

### Phase 1 Audit: Foundation Complete (after artefact 05)

Run after artefact 05 (Terminology) is approved, before starting artefact 06.

| Check | Question | Action if gap found |
|-------|----------|-------------------|
| Discovery → Architecture | Does every service, database, and external system in artefact 01 appear in artefact 02 diagrams? | Update artefact 02 |
| Discovery → Intent | Are all major modules from artefact 01 reflected in the business intent (artefact 03)? | Update artefact 03 or flag as open question |
| Architecture → Domains | Does every significant component in artefact 02 map to a domain in artefact 04? | Update artefact 04 |
| Domains → Terminology | Does every domain in artefact 04 have at least some terms defined in artefact 05? | Add missing terms |
| Consistency | Are domain names used consistently across artefacts 03, 04, and 05? | Align naming |

Present a brief summary:

```text
Phase 1 Audit (Foundation):
- Architecture coverage: all discovery items represented ✓
- Domain coverage: 5/5 domains have terminology defined ✓
- Consistency: 1 naming mismatch found (fixed)
Proceeding to Evidence Extraction phase.
```

---

### Phase 2 Audit: Evidence Extraction Complete (after artefact 09)

Run after artefact 09 (Communications) is approved, before starting artefact 10.

| Check | Question | Action if gap found |
|-------|----------|-------------------|
| Domain → Entity coverage | Does every domain in artefact 04 have at least one entity in artefact 06? | Investigate the domain's code for missing entities |
| Entity → Rule coverage | Does every entity with lifecycle states have state transition rules in artefact 07? | Add missing rules |
| Integration → Rules | Does every integration in artefact 08 have at least one failure-handling rule in artefact 07? | Add missing rules |
| Domain → Integration coverage | Are all external systems from artefact 02 documented in artefact 08? | Investigate and add missing integrations |
| Communication triggers | Does every communication in artefact 09 have a triggering rule or process identified? | Link to rules or flag as open question |
| Discovery back-check | Are there modules/services from artefact 01 that have produced zero entities, zero rules, and zero integrations? | Investigate those modules |

Present summary:

```text
Phase 2 Audit (Evidence Extraction):
- Entities per domain: all domains covered ✓
- State transition rules: 2 entities missing transition rules (ENT-005, ENT-008) — added BR-019, BR-020
- Integration failure rules: 1 gap (INT-003 has no failure rule) — added BR-021
- Undocumented modules: "shared/reconciliation" not yet analysed — flagged for process phase
Proceeding to Process & Specification phase.
```

---

### Phase 3 Audit: Process & Specification Complete (after artefact 12)

Run after artefact 12 (Functional Spec) is approved, before starting artefact 13.

| Check | Question | Action if gap found |
|-------|----------|-------------------|
| Process → Rules | Does every process stage reference the relevant business rules from artefact 07? | Update process details (artefact 11) |
| Process → Integrations | Are all integrations from artefact 08 referenced in at least one process step? | Update process model or flag if unused |
| Process → Communications | Are all communications from artefact 09 attached to a process step? | Update process details |
| Functional Spec → Entities | Does the functional spec cover all entities from artefact 06? | Add missing functional areas |
| Functional Spec → Rules | Are all business rules from artefact 07 referenced in at least one functional requirement? | Add missing requirements |
| Discovery back-check | Anything from artefact 01 still not addressed? | Investigate and add |

Present summary:

```text
Phase 3 Audit (Process & Specification):
- Rules referenced in processes: 28/30 (BR-015 and BR-022 are utility rules not in any process — confirmed OK)
- Integrations in process steps: 4/4 ✓
- Communications attached: 8/9 (COMMS-006 timing unclear — open question added)
- Functional spec entity coverage: 12/12 ✓
- Discovery items fully addressed: 16/16 ✓
Proceeding to Scenarios & Criteria phase.
```

---

### Phase 4 Audit: Scenarios & Criteria Complete (after artefact 16)

Run after artefact 16 (E2E Criteria) is approved, before starting artefact 17.

| Check | Question | Action if gap found |
|-------|----------|-------------------|
| Entity → Scenario coverage | Does every entity appear in at least one scenario (13 or 14)? | Add missing scenarios |
| Rule → Criteria coverage | Does every business rule have at least one acceptance criterion (15 or 16)? | Add missing criteria |
| Integration → Scenario coverage | Does every integration have both success and failure scenarios? | Add missing scenarios |
| Communication → Criteria | Does every communication have a criterion validating its trigger and content? | Add missing criteria |
| Process → E2E coverage | Does every major process in artefact 10 have at least one E2E scenario? | Add missing E2E scenarios |
| Permission coverage | Does every role/permission have at least one criterion? | Add missing criteria |
| Open question review | Compile all unresolved open questions and present to user | Ask user to resolve or defer |

Present summary:

```text
Phase 4 Audit (Scenarios & Criteria):
- Entities with scenarios: 12/12 ✓
- Rules with criteria: 28/30 (2 gaps — added AC-045, AC-046)
- Integrations with success+failure: 4/4 ✓
- Communications with criteria: 9/9 ✓
- E2E journey coverage: 3/3 ✓
- Permissions tested: 5/5 ✓
- Open questions remaining: 2 (user deferred)
Proceeding to Capability Map.
```

---

### Audit Recording in .progress.json

After each phase audit, update `.progress.json`:

```json
{
  "phaseAudits": {
    "phase1": {"completed": true, "date": "2026-07-01", "gapsFound": 1, "gapsResolved": 1},
    "phase2": {"completed": true, "date": "2026-07-01", "gapsFound": 3, "gapsResolved": 3},
    "phase3": {"completed": true, "date": "2026-07-02", "gapsFound": 1, "gapsResolved": 0, "gapsDeferred": 1},
    "phase4": {"completed": true, "date": "2026-07-02", "gapsFound": 2, "gapsResolved": 2}
  }
}
```

### Audit Behaviour Rules

- Phase audits are **mandatory** — do not skip them.
- If the user says "proceed" at a phase boundary, run the audit first, then proceed.
- If gaps are found, present them to the user before fixing. The user may say "skip" or "fix."
- If fixing, apply cascade updates to downstream artefacts affected by the fix.
- Phase audits should be lightweight — read artefact headers and ID lists, not full file contents. Use `grepSearch` to count IDs across files.
