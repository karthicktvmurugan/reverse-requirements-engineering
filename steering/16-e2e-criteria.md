---
inclusion: manual
---
# E2E Acceptance Criteria — Steering Guide

## Dependencies
Read these artefacts before producing this one:
- 14 E2E Scenario Catalogue
- 15 Acceptance Criteria

## Load These References
- None required (this artefact synthesises from prior artefacts already read)

---

## What to Produce

**Folder:** `16-e2e-acceptance-criteria/` (one file per E2E scenario)

**Purpose:** Define precise, testable acceptance criteria for each end-to-end scenario. These criteria validate the complete journey, not just individual stages.

**Format:** Write acceptance criteria as **precise, plain English statements**. Do NOT use Given/When/Then format. Each criterion should be traceable to a specific E2E scenario.

**Each file (one per E2E scenario) should include:**

- E2E Scenario reference (e.g. `E2E-001`)
- Scenario summary (one line)

**Acceptance criteria grouped by:**

- **Happy path criteria** — what must be true when the entire journey completes successfully
- **Cross-stage criteria** — validations that span multiple stages (e.g. data consistency, timing)
- **Exception path criteria** — what must happen when failures occur at different points in the journey
- **Recovery criteria** — what must happen when a failed journey is retried or resumed
- **Data integrity criteria** — end-state data validation after full journey completion
- **Timing and ordering criteria** — sequence requirements across the journey
- **Role and permission criteria** — authorisation requirements that span multiple stages
- **Integration criteria** — external system interactions across the full journey
- **Notification criteria** — communications expected at various journey points

**Example format:**

```text
## E2E-001: Standard Property Settlement — Happy Path

AC-E2E-001-01: When a settlement workspace is created and all parties complete financial preparation, the system must reserve funds from the nominated bank account before proceeding to lodgement.

AC-E2E-001-02: The total disbursement amounts across all parties must equal the total funds reserved, with no unaccounted remainder after settlement execution.

AC-E2E-001-03: If the land registry lodgement is rejected after funds have been reserved, the system must release all reserved funds within 30 seconds and notify all affected parties.

AC-E2E-001-04: The settlement cannot proceed to execution unless all prerequisite stages (financial preparation, approval, funds reservation, lodgement) have completed successfully in order.
```

**Traceability:**

- Each criterion must reference the E2E scenario ID
- Each criterion should reference related per-stage acceptance criteria (`AC` IDs from artefact 15) where applicable
- Each criterion should reference business rules (`BR` IDs), integrations (`INT` IDs), and entities (`ENT` IDs) validated

The folder must contain a `README.md` listing all E2E acceptance criteria files with their scope.

---

## Definition of Done

Done when:

- each criterion has an `AC-E2E` ID,
- criteria are written as precise, plain English statements (not Given/When/Then),
- criteria are grouped by E2E scenario,
- happy path, exception path, recovery, data integrity, timing, role, integration, and notification criteria are considered,
- each criterion references an E2E scenario ID,
- cross-stage validations are covered (e.g. data consistency end-to-end),
- criteria validate the full journey outcome, not just individual steps.

**Do not proceed if:**

- criteria only validate individual stages (that is what artefact 15 is for),
- criteria are not traceable to E2E scenarios,
- cross-stage and journey-level validations are missing,
- failure recovery across stages is not tested.
