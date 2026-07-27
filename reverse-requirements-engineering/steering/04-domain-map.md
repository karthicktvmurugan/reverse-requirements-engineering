---
inclusion: manual
---
# Domain Map — Steering Guide

## Dependencies
Read these artefacts before producing this one:
- 01 Codebase Discovery Report
- 02 Architecture Diagram
- 03 Business Intent Hypothesis

## Load These References
- `steering/inspection-method.md` (Stage 3: Business Domain Discovery)

---

## What to Produce

**File:** `04-domain-map.md`

**Purpose:** Identify the bounded contexts, sub-domains, and their relationships within the system. This provides the conceptual architecture — how business concepts are organised.

**Must include (do not skip any part):**

**Domain Relationship Diagram:**

- All identified bounded contexts / sub-domains as nodes
- Relationships between domains (upstream/downstream, shared kernel, customer/supplier, conformist, anti-corruption layer)
- Data flow direction between domains
- Shared concepts that cross domain boundaries
- Rendered as a Mermaid diagram

**Per domain, document:**

- Domain name (business language, not technical module name)
- Core responsibility / business purpose
- Key entities owned by this domain
- Key business events produced or consumed
- Upstream dependencies (what it needs from other domains)
- Downstream consumers (who depends on it)
- Integration pattern used at boundaries (API, event, shared DB, file)
- Code modules / packages that implement this domain
- Confidence level

**Should also include:**

- Context mapping patterns observed (anti-corruption layers, shared kernels, open host services)
- Ambiguous boundaries or overlapping responsibilities (flag as open questions)
- Alignment or misalignment between code structure and domain structure
- Code evidence references
- Mermaid diagrams must show ALL discovered domains — do not omit any for brevity

---

## Definition of Done

Done when:

- all bounded contexts are identified with `DOM` IDs,
- domain relationship diagram is produced showing ALL domains and their relationships,
- per-domain responsibilities are documented,
- upstream/downstream dependencies are mapped,
- integration patterns at domain boundaries are noted,
- context mapping patterns are identified where observable,
- code modules implementing each domain are listed.

**Do not proceed if:**

- known modules are not mapped to a domain,
- diagram omits discovered domains,
- relationships between domains are not shown,
- shared concepts crossing boundaries are not flagged.
