---
inclusion: manual
---
# Business Scenario Catalogue — Steering Guide

## Dependencies
Read these artefacts before producing this one:
- 12 Functional Specification

## Load These References
- None required (this artefact synthesises from prior artefacts already read)

---

## What to Produce

**Folder:** `13-business-scenario-catalogue/` (one file per scenario group)

**Purpose:** Convert functional and process understanding into testable business scenarios at the stage level.

**Should include per scenario:**

- Scenario ID (`SCN` prefix)
- Scenario name
- Business objective
- Preconditions
- Actors
- Trigger
- Test data needs
- Main flow
- Alternate flows
- Expected business outcome
- Related business rules (reference `BR` IDs)
- Related entities (reference `ENT` IDs)
- Related integrations (reference `INT` IDs)
- Related notifications or communications (reference `COMMS` IDs)
- Source evidence

**Scenario types to consider:**

- Happy path / positive scenarios
- Negative / validation failure scenarios
- Boundary value scenarios
- Permission / role-based scenarios
- Integration success and failure scenarios
- Notification trigger scenarios
- State transition scenarios
- Exception and recovery scenarios
- Timing and scheduling scenarios

**Organisation:**

- Group scenarios by process stage or functional area
- One file per scenario group
- Each file should cover a cohesive set of related scenarios
- The folder must contain a `README.md` listing all scenario files with their scope

---

## Definition of Done

Done when:

- each scenario has an `SCN` ID,
- scenarios include preconditions, actors, trigger, test data needs, main flow, alternate flows, and expected business outcome,
- positive, negative, boundary, permission, integration, and notification scenarios are considered,
- scenarios reference `FUNC`, `PROC`, `BR`, `ENT`, `INT`, and `COMMS` IDs where relevant.

**Do not proceed if:**

- scenarios are just restatements of functions,
- test data needs are missing,
- negative and alternate scenarios are not considered,
- there is no traceability to prior artefacts.
