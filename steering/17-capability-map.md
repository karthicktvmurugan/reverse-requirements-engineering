---
inclusion: manual
---
# Capability Map — Steering Guide

## Dependencies
Read these artefacts before producing this one:
- All prior artefacts (01 through 16)

## Load These References
- None required

---

## What to Produce

**File:** `17-capability-map.md`

**Purpose:** Provide a high-level view of business capabilities derived from the analysis, mapped against an industry or organisational capability reference.

**Capability Map Source — Reference Framework Protocol:**

Before producing this artefact, the agent must:

1. **Ask the user** if they have a capability map reference to provide (e.g. an existing enterprise capability map, industry reference model, or organisational standard).
2. **If the user provides one** — use it as the reference framework and map discovered capabilities against it.
3. **If the user does not have one** — search the internet for a relevant industry capability map or reference model (e.g. BIAN for banking, TMForum for telecoms, APQC for general business processes) and propose it to the user for confirmation before proceeding.

**The capability map should show:**

- Business capabilities discovered in the codebase (reference `CAP` IDs)
- Mapping to reference framework (if available)
- Capability maturity or coverage level inferred from code
- Gaps between reference and implementation
- Evidence references linking capabilities to functional requirements (`FUNC` IDs), processes (`PROC` IDs), and entities (`ENT` IDs)

**Organisation:**

- Group capabilities by business domain (from artefact 04)
- Show hierarchy (L0 → L1 → L2 capabilities where applicable)
- Indicate which capabilities are fully implemented vs partially implemented vs referenced but not implemented
- Flag capabilities in the reference framework that have no corresponding implementation

---

## Definition of Done

Done when:

- user has been asked for capability map reference,
- capabilities are mapped against a reference framework (user-provided or researched),
- discovered capabilities are listed with evidence,
- gaps between reference and implementation are identified.

**Do not consider the overall analysis complete if:**

- critical rules lack acceptance criteria,
- critical integrations lack success/failure scenarios,
- notifications lack acceptance criteria expectations,
- important user roles lack permission scenarios,
- E2E journey scenarios lack cross-stage acceptance criteria,
- open questions materially affect scope.
