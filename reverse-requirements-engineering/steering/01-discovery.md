---
inclusion: manual
---
# Codebase Discovery Report — Steering Guide

## Dependencies
Read these artefacts before producing this one:
- None (this is the first artefact)

## Load These References
- `steering/inspection-method.md` (Stages 1, 2, 3 are primary)
- `steering/technology-checklists.md` (select relevant technology type)

---

## What to Produce

**File:** `01-codebase-discovery-report.md`

**Purpose:** Establish what the system(s) appear to be, how they are structured, and where business logic is likely located.

**Should include:**

- Repository overview (one section per repo if multiple)
- Application type
- Entry points
- Major modules or services
- External integrations
- Data stores and schemas
- APIs, jobs, events, or user interfaces
- Likely business domains
- Key files requiring deeper analysis
- Initial risks, gaps, and unknowns
- Cross-repo relationships (if multiple repos analysed together)

**Do not include yet:**

- Full functional specification
- Acceptance criteria
- Detailed business scenarios

---

## Definition of Done

Done when:

- repository structure is summarised,
- application type is identified,
- major modules and entry points are listed,
- likely business domains are identified,
- data stores and integration points are noted,
- testing strategy is observed,
- key areas for deeper analysis are prioritised,
- major unknowns are recorded.

**Do not proceed if:**

- the agent has not identified the main entry points,
- the agent cannot explain what type of system this is,
- the agent has not listed likely business-relevant modules.
