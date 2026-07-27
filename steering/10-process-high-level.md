---
inclusion: manual
---
# High-Level Process Model — Steering Guide

## Dependencies
Read these artefacts before producing this one:
- 06 Data Model and Entity Catalogue
- 07 Business Rules Catalogue
- 08 External Integration Contract Summary
- 09 Notification and Communication Catalogue

## Load These References
- `steering/inspection-method.md` (Stage 4: Entry Point to Outcome Flow Tracing)

---

## What to Produce

**Folder:** `10-high-level-process-model/` (one file per process — always)

**Purpose:** Describe the end-to-end business processes supported by the system.

Each process file should include:

- Process overview
- Actors and systems involved
- Triggering events
- Major process stages
- Key decision points
- Successful outcomes
- Alternate outcomes
- Failure paths
- External integration touchpoints
- Notification and communication touchpoints
- Suggested process diagram in Mermaid syntax — must include ALL stages, decision points, and actors; do not simplify or omit steps for brevity

The folder must contain a `README.md` listing all process files.

**Mermaid Diagram Requirements:**

- Use flowchart or sequence diagram syntax as appropriate
- Include ALL actors, stages, decision points, and outcomes
- Show both success and failure paths
- Label decision points with the condition being evaluated
- Show integration and notification touchpoints
- If a process is too complex for one diagram, split into multiple focused diagrams rather than omitting elements
- Each diagram must have a title indicating what it covers

---

## Definition of Done

Done when:

- major end-to-end processes have `PROC` IDs,
- each process has its own file,
- actors, systems, triggers, stages, decision points, outcomes, and failure paths are described,
- integration and communication touchpoints are shown,
- Mermaid diagrams include ALL stages, decision points, and actors without omission,
- links to functional requirements and rules are present.

**Do not proceed if:**

- the process is just a technical call stack,
- decision points are missing,
- alternate or failed outcomes are ignored,
- diagrams are simplified by omitting steps.
