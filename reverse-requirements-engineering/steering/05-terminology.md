---
inclusion: manual
---
# Terminology and Data Dictionary — Steering Guide

## Dependencies
Read these artefacts before producing this one:
- 03 Business Intent Hypothesis
- 04 Domain Map

## Load These References
- `steering/inspection-method.md` (Stage 3: Business Domain Discovery)

---

## What to Produce

**File:** `05-terminology-and-data-dictionary.md`

**Purpose:** Define the precise meaning of business and technical terms used within the system, resolving ambiguity before detailed entity and rule analysis begins.

**Should include:**

For each term:

- Term name
- Domain (which bounded context owns this term)
- Business definition (plain English, not technical)
- Synonyms or aliases found in code (different names for the same concept)
- Disambiguation (if the same word means different things in different contexts)
- Related terms
- Source evidence (where in code this term appears)
- Confidence level

**Organisation:**

- Group terms by domain (from artefact 04)
- Highlight terms where the same word is used inconsistently across the codebase
- Flag terms where business meaning is unclear or assumed
- Include abbreviations and acronyms found in code with their expansions

**Should also include:**

- Status/state value definitions (what does each status value mean in business terms)
- Enum value definitions (what do code enum values represent)
- Configuration key explanations (what do key config values control)

---

## Definition of Done

Done when:

- key terms are defined with `TERM` IDs,
- terms are grouped by domain (from artefact 04),
- synonyms and aliases are captured,
- ambiguous terms are flagged and disambiguated,
- status and enum values are explained in business terms,
- abbreviations and acronyms are expanded,
- source evidence is linked for each term.

**Do not proceed if:**

- key business terms used in code are not defined,
- same word used differently in different contexts is not disambiguated,
- status/state values remain unexplained.
