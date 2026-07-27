---
inclusion: manual
---
# Low-Level Process Details — Steering Guide

## Dependencies
Read these artefacts before producing this one:
- 10 High-Level Process Model

## Load These References
- `steering/inspection-method.md` (Stage 4: Entry Point to Outcome Flow Tracing)

---

## What to Produce

**Folder:** `11-low-level-process-details/` (one file per major process stage)

**Purpose:** Break each high-level process stage into detailed steps.

**Should include:**

- Step-by-step flow
- Actor/system responsibility per step
- Inputs and outputs per step
- Rules and validations applied
- Data created or updated
- External requests sent and responses handled
- Notifications, alerts, emails, or messages generated
- Downstream effects
- Exceptions and recovery paths
- Code evidence references

**Per step, document:**

- Step number within the stage
- Actor or system performing the step
- Action description in business language
- Input data or preconditions for the step
- Business rules applied (reference `BR` IDs)
- Data entities created or modified (reference `ENT` IDs)
- Integrations triggered (reference `INT` IDs)
- Communications generated (reference `COMMS` IDs)
- Output or result of the step
- Exception paths and recovery behaviour
- Code evidence reference

The folder must contain a `README.md` listing all process detail files with their scope.

---

## Definition of Done

Done when:

- each major process is decomposed into detailed steps,
- each step identifies actor/system responsibility,
- inputs, outputs, data changes, rules, integrations, notifications, and exceptions are captured,
- step-level traceability to code and rules is present.

**Do not proceed if:**

- low-level steps do not show responsibility or outcome,
- business rules are not attached to the relevant step,
- exception paths are omitted.
