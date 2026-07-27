---
inclusion: manual
---
# E2E Scenario Catalogue — Steering Guide

## Dependencies
Read these artefacts before producing this one:
- 10 High-Level Process Model
- 13 Business Scenario Catalogue

## Load These References
- None required (this artefact synthesises from prior artefacts already read)

---

## What to Produce

**Folder:** `14-e2e-scenario-catalogue/` (one file per E2E scenario)

**Purpose:** Define end-to-end scenarios that span the entire business journey — from initial trigger through all process stages to final business outcome. Unlike the per-stage business scenarios in artefact 13, these scenarios trace a complete path through the system.

**Each E2E scenario file should include:**

- E2E Scenario ID (format: `E2E-001`, `E2E-002`, etc.)
- Scenario name
- Business objective of the entire journey
- Actors involved across all stages
- Preconditions at journey start
- Trigger that initiates the journey

**Steps (ordered sequence across the full journey):**

Steps must be ordered as a **realistic test simulation sequence** — a tester should be able to follow the steps in numbered order to execute the scenario end-to-end. Each step's preconditions must be satisfied by preceding steps. This means:

- Steps represent the order a user/tester would execute them, NOT just a logical dependency graph.
- Where steps can happen in any order, pick a valid simulation order and note the flexibility in a "Step Ordering Notes" section.
- Respect system constraints (e.g., if action X requires status Y, ensure a prior step achieves status Y).
- Clearly mark which actor (user, external system, or business system) performs each step.

For each step in the E2E scenario:

- Step number
- Stage or process area (referencing `10-high-level-process-model/`)
- Actor or system performing the step
- Action performed
- Input data or conditions
- Expected outcome of the step
- Business rules applied (referencing `BR` IDs)
- Integrations triggered (referencing `INT` IDs)
- Notifications generated (referencing `COMMS` IDs)
- Data created or changed (referencing `ENT` IDs)
- Decision points and branching conditions

**Journey-level information:**

- Expected final business outcome (success path)
- Alternate journey paths (variations, exceptions)
- Failure points and recovery paths across the journey
- Cross-stage dependencies (what must complete before the next stage can begin)
- Test data requirements for the full journey
- Related per-stage scenarios (referencing `SCN` IDs from artefact 13)
- Related functional requirements (referencing `FUNC` IDs)
- Source evidence

The folder must contain a `README.md` listing all E2E scenario files with their scope.

---

## Definition of Done

Done when:

- each E2E scenario has an `E2E` ID,
- scenarios trace the complete journey from trigger to final business outcome,
- steps are ordered and span all relevant process stages,
- each step identifies actor, action, inputs, outcomes, rules, integrations, and notifications,
- cross-stage dependencies are documented,
- alternate and failure paths across the journey are described,
- test data requirements for the full journey are identified,
- scenarios reference per-stage scenarios (`SCN` IDs) and process steps (`PROC` IDs).

**Do not proceed if:**

- E2E scenarios only cover a single stage (that is what artefact 13 is for),
- steps are missing across key stages,
- steps are not in a valid simulation order (a tester cannot follow them sequentially),
- cross-stage dependencies are not documented,
- failure paths at different journey points are not considered.
