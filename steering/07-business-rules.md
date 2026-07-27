---
inclusion: manual
---
# Business Rules Catalogue — Steering Guide

## Dependencies
Read these artefacts before producing this one:
- 05 Terminology and Data Dictionary
- 06 Data Model and Entity Catalogue

## Load These References
- `steering/inspection-method.md` (Stage 6: Business Rule Extraction)
- `steering/technology-checklists.md` (select relevant technology type)

---

## What to Produce

**Folder:** `07-business-rules-catalogue/` (one file per rule domain if large)

**Purpose:** Extract and document the decision logic, validations, thresholds, constraints, and policies implemented in code.

**Deep analysis required:** Go deeper than surface-level validations. Specifically look for:

- **Validation rules** — field-level, cross-field, and cross-entity validations
- **Reconciliation rules** — matching, balancing, and settlement logic
- **State transition rules** — guards, preconditions, and allowed transitions
- **Calculation rules** — derived values, fees, interest, apportionment
- **Timing rules** — cut-offs, deadlines, scheduling constraints, expiry
- **Authorisation rules** — who can do what, under what conditions, with what limits
- **Exception and override rules** — when normal rules can be bypassed and by whom

**Should include per rule:**

- Business rule ID
- Rule name
- Rule description in business language
- Trigger or context where the rule applies
- Inputs used by the rule
- Decision logic or condition
- Outcome when the rule passes
- Outcome when the rule fails
- Error messages, status codes, or user-facing messages where observable
- Rule source such as validation, service logic, workflow, tests, schema, configuration, or feature flag
- Dependencies on data, roles, dates, external systems, or configuration
- Confidence level and evidence type
- Open questions for unclear business intent

---

## Definition of Done

Done when:

- each rule has a `BR` ID,
- each rule has trigger/context, condition, pass outcome, and fail outcome,
- rule source is linked to code evidence,
- validations, permissions, thresholds, lifecycle guards, reconciliation rules, and exception rules are considered,
- state transition requirements are documented,
- calculation and timing rules are captured,
- user-facing or API-facing error messages are captured where observable.

**Do not proceed if:**

- rules are mixed into prose without IDs,
- rules do not include pass/fail outcomes,
- negative paths and boundary rules are skipped,
- reconciliation and state transition rules are not explored,
- calculation logic is ignored.
