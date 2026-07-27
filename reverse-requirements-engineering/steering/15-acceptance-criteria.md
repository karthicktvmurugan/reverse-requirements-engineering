---
inclusion: manual
---
# Acceptance Criteria — Steering Guide

## Dependencies
Read these artefacts before producing this one:
- 13 Business Scenario Catalogue
- 14 E2E Scenario Catalogue

## Load These References
- None required (this artefact synthesises from prior artefacts already read)

---

## What to Produce

**Folder:** `15-acceptance-criteria/` (one file per criteria group)

**Purpose:** Define precise, testable acceptance criteria for per-stage scenarios suitable for UAT execution.

**Format:** Write acceptance criteria as **precise, plain English statements**. Do NOT use Given/When/Then format.

**Example format:**

```text
AC-001: The system must reject a funds reservation request when the requested amount exceeds the available balance.

AC-002: When a reservation is cancelled after funds have been reserved, the reserved funds must be released back to the available balance within 30 seconds.

AC-003: Only users with the "reservation-approver" role can authorise a reservation above £500.
```

**Should include:**

- Feature or scenario reference
- Acceptance criteria as clear, precise English statements
- Positive cases
- Negative cases
- Boundary cases
- Role/permission cases
- Data validation cases
- Business rule cases
- External integration success and failure cases where relevant
- Notification, alert, email, and communication expectations where relevant
- Exception cases
- Audit, reporting, or downstream system expectations where relevant
- Traceability to scenarios, business rules, entities, integrations, notifications, and code evidence

**Per criterion:**

- `AC` ID
- Plain English statement of what must be true or what the system must do
- Related scenario (`SCN` ID)
- Related business rules (`BR` IDs)
- Related entities (`ENT` IDs)
- Related integrations (`INT` IDs)
- Related communications (`COMMS` IDs)
- Evidence reference

The folder must contain a `README.md` listing all acceptance criteria files with their scope.

---

## Definition of Done

Done when:

- each acceptance criterion has an `AC` ID,
- criteria are written as precise, plain English statements (not Given/When/Then),
- each criterion is testable by a business user or UAT tester,
- criteria cover positive, negative, boundary, permission, integration, notification, and exception cases where relevant,
- each criterion references at least one scenario or functional requirement,
- expected user-visible outcome and/or business outcome is clear.

**Do not proceed if:**

- acceptance criteria are technical unit-test assertions,
- criteria cannot be executed or observed in UAT,
- criteria are not linked to scenarios or functions,
- criteria ignore business rules, integrations, or notifications discovered earlier.
