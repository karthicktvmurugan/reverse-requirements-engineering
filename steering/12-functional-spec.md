---
inclusion: manual
---
# Functional Specification — Steering Guide

## Dependencies
Read these artefacts before producing this one:
- 06 Data Model and Entity Catalogue
- 07 Business Rules Catalogue
- 08 External Integration Contract Summary
- 09 Notification and Communication Catalogue
- 10 High-Level Process Model
- 11 Low-Level Process Details

## Load These References
- `steering/inspection-method.md` (Stage 4: Entry Point to Outcome Flow Tracing)

---

## What to Produce

**Folder:** `12-functional-specification/` (one file per functional area if large)

**Purpose:** Document observable system behaviour in business-readable terms, organised by process context.

**Should include:**

- Functional areas (organised by process stage where applicable)
- Features or capabilities
- Inputs
- Outputs
- Validations
- Business rules, referencing `07-business-rules-catalogue/`
- Business entities, referencing `06-data-model-and-business-entity-catalogue.md`
- State transitions
- Error and exception handling
- Permissions or role-based behaviour
- External integrations, referencing `08-external-integration-contract-summary.md`
- Notifications, referencing `09-notification-alert-and-communication-catalogue.md`
- Reports, files, or messages
- Code evidence references

**Per functional requirement:**

- `FUNC` ID
- Feature or capability name
- Business description (what the system does, not how it does it technically)
- Trigger or entry point
- Actors and roles involved
- Preconditions
- Inputs and their validation
- Business rules applied (reference `BR` IDs)
- Data entities involved (reference `ENT` IDs)
- State transitions triggered
- Integrations called (reference `INT` IDs)
- Notifications generated (reference `COMMS` IDs)
- Success outcome
- Failure/exception outcomes (reference `ERR` IDs)
- Permissions required
- Code evidence

The folder must contain a `README.md` listing all functional specification files with their scope.

---

## Definition of Done

Done when:

- each major function has a `FUNC` ID,
- functions are grouped by business capability or process stage,
- inputs, outputs, validations, rules, permissions, state changes, integrations, notifications, and errors are described,
- related `ENT`, `BR`, `INT`, `COMMS`, and `ERR` IDs are referenced,
- code evidence is linked.

**Do not proceed if:**

- functions are described as technical methods rather than business behaviours,
- rules/entities/integrations/communications are not cross-referenced,
- error and alternate behaviours are skipped.
