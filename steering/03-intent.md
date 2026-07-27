---
inclusion: manual
---
# Business Intent Hypothesis — Steering Guide

## Dependencies
Read these artefacts before producing this one:
- 01 Codebase Discovery Report
- 02 Architecture Diagram

## Load These References
- `steering/inspection-method.md` (Stage 3: Business Domain Discovery)

---

## What to Produce

**File:** `03-business-intent-hypothesis.md`

**Purpose:** Infer what business problem the system(s) appear to solve.

**Should include:**

- Product or capability summary
- Primary users or actors
- Business objectives inferred from code
- Core business outcomes
- Value delivered to users or downstream systems
- Key business events or lifecycle moments
- Supporting code evidence (from all analysed repos)
- Confidence rating per inference
- Open questions for product/business stakeholders

---

## Definition of Done

Done when:

- likely business purpose is described,
- actors or users are identified,
- business capabilities are listed,
- primary business outcomes are described,
- confidence levels are assigned,
- assumptions and open questions are recorded.

**Do not proceed if:**

- the agent cannot distinguish confirmed behaviour from hypothesis,
- the business purpose is described only in technical terms,
- no stakeholder validation questions are produced.
