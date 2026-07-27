---
inclusion: manual
---
# Data Model and Entity Catalogue — Steering Guide

## Dependencies
Read these artefacts before producing this one:
- 04 Domain Map
- 05 Terminology and Data Dictionary

## Load These References
- `steering/inspection-method.md` (Stage 5: Data and State Analysis)
- `steering/technology-checklists.md` (select relevant technology type)

---

## What to Produce

**File:** `06-data-model-and-business-entity-catalogue.md`

**Purpose:** Identify the key data structures and translate them into business entities and relationships.

**Should include:**

- Data stores, schemas, migrations, models, DTOs, events, and payload structures analysed
- Business entities inferred from the codebase(s)
- Entity descriptions in business language (using terms defined in artefact 05)
- Key attributes and their likely business meaning
- Mandatory versus optional fields where observable
- Identifiers, references, and relationships between entities
- Entity lifecycle states or status fields
- Derived, calculated, or system-generated fields
- Sensitive, regulated, or audit-relevant data indicators where observable
- Data ownership and source of truth where inferable
- Data quality rules discovered in validations or tests
- Mermaid ER diagram or entity relationship summary — must show ALL entities and relationships, splitting into multiple diagrams if needed
- Code evidence references (with repo prefix for multi-repo)
- Assumptions and open questions

**ER Diagram Requirements:**

- Every discovered entity must appear in at least one ER diagram
- Show cardinality on all relationships (1:1, 1:N, M:N)
- If too large for one Mermaid block, split into multiple focused diagrams by domain
- Label relationship lines with the nature of the relationship
- Include key attributes within each entity box

---

## Definition of Done

Done when:

- key entities are listed with `ENT` IDs,
- entity descriptions use terminology from artefact 05,
- important attributes are listed with business meaning,
- relationships are described,
- lifecycle states are identified where present,
- data constraints and validation hints are captured,
- source-of-truth assumptions are recorded,
- ER diagram(s) show ALL entities and relationships (split into multiple diagrams if needed),
- entity evidence is linked to schemas, models, migrations, DTOs, events, or tests.

**Do not proceed if:**

- business entities are only copied from table or class names without interpretation,
- lifecycle states are ignored,
- relationships are not considered,
- ER diagram omits discovered entities,
- key data gaps are not recorded.
