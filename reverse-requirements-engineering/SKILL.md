---
name: reverse-requirements-engineering
description: Analyse one or more codebases and progressively reverse-engineer business-facing artefacts — from discovery through to traceable acceptance criteria. Supports new team onboarding, legacy modernisation, test strategy development, team handovers, and feature impact assessment where documentation does not exist.
license: MIT
metadata:
  author: karthicktvmurugan
  version: "5.0"
---

# Reverse Requirements Engineering Skill

## Purpose

This skill helps an AI coding agent analyse one or more codebases and progressively reverse-engineer business-facing artefacts — from discovery through to traceable acceptance criteria.

The goal is not to summarise code mechanically. The goal is to infer, validate, and document the product's business intent, functional behaviour, process flow, business scenarios, and user-testable acceptance criteria.

The AI agent must work iteratively. It should produce one artefact at a time, pause for human review, incorporate feedback, and only then proceed to the next artefact.

## When to Use This Skill

This skill is designed for scenarios where structured documentation does not exist and must be recovered from source code. Common use cases include:

1. **New team member onboarding** — A business analyst, developer, or QA engineer joins a team with no existing documentation. The skill produces structured business-readable artefacts that accelerate understanding from weeks to hours.

2. **Legacy modernisation** — Before rebuilding or migrating a system, teams need a complete "as-is" reference. The skill produces functional specs, business rules, and process models that serve as the baseline for gap analysis against a target architecture.

3. **Test strategy development** — Beyond UAT, the artefacts support integration test design (from integration contracts), regression scope definition (from scenarios), performance test targeting (from process models), and failure injection planning (from E2E failure paths).

4. **Handover between teams or vendors** — When responsibility for a system transfers (vendor exit, team reorganisation, offshoring), the artefacts provide structured knowledge transfer with traceability IDs that create a shared vocabulary.

5. **Feature impact assessment** — Before building new features, understanding what currently exists. Business rule IDs, entity relationships, and integration contracts allow teams to trace the blast radius of proposed changes.

---

## Multi-Repo Scaffolding and Folder Management

### Core Principle

Each time the skill is invoked, artefacts are produced inside a named output folder under `artefacts/`. The agent must always ask the user how to organise outputs — never assume.

### First Invocation Flow

When the skill is invoked for the first time (no existing output folders), or the user provides one or more repos that have not been analysed before:

1. **Ask the user to confirm the output folder name.**
   - Suggest a folder name derived from the repos or business domain (kebab-case).
   - Example: If repos is `trip-recommendation-engine-poc', suggest `trip-recommendation` or similar.
   - Wait for confirmation or a custom name from the user.

2. **If multiple repos are provided, ask how to organise them:**
   - Option A: **Analyse together** — All repos are treated as related from a requirements perspective. Artefacts draw evidence from all repos and produce a single unified output set in one folder.
   - Option B: **Analyse separately** — Each repo gets its own folder. Ask which repo to analyse first. Instruct the user to invoke the skill again for the remaining repos.

3. **Create the output folder** at `artefacts/{confirmed-folder-name}/` and begin analysis.

### Subsequent Invocation Flow (Skill Re-invoked)

When the skill is invoked and one or more output folders already exist:

1. **Check if a previous workflow is incomplete** (not all artefacts produced). If so:
   - Resume from the last completed artefact and continue the workflow.
   - Do not re-ask folder questions — use the existing folder.

2. **If a repo is provided again that was previously analysed:**
   - Ask the user whether to:
     - Option A: **Create a new folder** — Start fresh analysis in a new named folder.
     - Option B: **Update existing folder** — Integrate new findings into existing artefacts.
   - If updating existing folder:
     - Update existing artefacts with delta comparison (see Delta Comparison below).
     - After updating, continue producing any remaining artefacts not yet generated.

3. **If a completely new repo is provided:**
   - Ask the user whether to:
     - Option A: **Create a new folder** — Start fresh analysis in a new named folder.
     - Option B: **Add to an existing folder** — Integrate new repo analysis into an existing artefact set.
   - If adding to existing folder, ask:
     - Option A: **Update existing artefacts** — Perform delta comparison and merge new findings.
     - Option B: **Create new artefacts** — Produce a new parallel set of artefacts within the same folder.

### Delta Comparison and Update Logic

When the user chooses to update existing artefacts with findings from a new repo, the agent categorises changes (updated requirements, new features, refined rules, new entities/processes/scenarios), presents a delta summary for approval, applies changes while preserving existing IDs and confidence levels, and performs a cascade review of all affected artefacts.

See `steering/protocols/delta-updates.md` for the full step-by-step delta comparison protocol.

### Output Folder Structure

```
artefacts/
├── {folder-name}/
│   ├── .progress.json
│   ├── 01-codebase-discovery-report.md
│   ├── 02-architecture-diagram.md
│   ├── 03-business-intent-hypothesis.md
│   ├── 04-domain-map.md
│   ├── 05-terminology-and-data-dictionary.md
│   ├── 06-data-model-and-business-entity-catalogue.md
│   ├── 07-business-rules-catalogue/
│   │   └── (one file per rule domain if large)
│   ├── 08-external-integration-contract-summary.md
│   ├── 09-notification-alert-and-communication-catalogue.md
│   ├── 10-high-level-process-model/
│   │   └── (one file per process)
│   ├── 11-low-level-process-details/
│   │   └── (one file per major process stage)
│   ├── 12-functional-specification/
│   │   └── (one file per functional area if large)
│   ├── 13-business-scenario-catalogue/
│   │   └── (one file per scenario group)
│   ├── 14-e2e-scenario-catalogue/
│   │   └── (one file per E2E scenario)
│   ├── 15-acceptance-criteria/
│   │   └── (one file per criteria group)
│   └── 16-e2e-acceptance-criteria/
│       └── (one file per E2E scenario)
└── README.md  ← index of all analysed repos and their output folders
```

### Splitting Artefacts Into Multiple Files

When any artefact becomes too long for a single file (generally over 300-400 lines), split it into multiple files within a subfolder. Apply this logic to:

- **Business Rules Catalogue** — split by rule domain (e.g. lifecycle rules, permission rules)
- **Functional Specification** — split by functional area or capability
- **High-Level Process Model** — one file per process (always)
- **Low-Level Process Details** — one file per major process stage
- **Business Scenario Catalogue** — one file per scenario group
- **E2E Scenario Catalogue** — one file per E2E scenario
- **Acceptance Criteria** — one file per criteria group
- **E2E Acceptance Criteria** — one file per E2E scenario

Each subfolder must contain a `README.md` index file listing all files in that folder with their scope.

### Writing Output in Smaller Chunks

To avoid write errors and unresponsive behaviour:

- **Never write more than 200 lines in a single file write operation.**
- If an artefact exceeds 200 lines, write it in chunks using append operations.
- If a single artefact is very large, prefer splitting into multiple files over writing one enormous file.
- This applies to all artefact creation and update operations.

### Artefacts README Index

The `artefacts/README.md` file must be created or updated on each invocation. It should contain:

- List of output folders with their creation date and scope.
- Which repos contributed to each folder.
- Current status (in progress, complete, under review).
- Last completed artefact (for workflow resumption).
- Links to key artefacts within each folder.

---

## Agent Role

When this skill is invoked, the agent must act as a technical lead and business analyst pair.

The agent is responsible for:

- reading and interpreting the codebase(s),
- identifying business meaning from technical implementation,
- separating evidence from inference,
- producing structured markdown artefacts,
- preserving traceability from code to acceptance criteria,
- collaborating with the human reviewer iteratively,
- managing multi-repo analysis and incremental updates,
- ensuring cascade consistency when artefacts are updated.

The agent must not behave like a generic code summariser. It must behave like an experienced analyst reverse-engineering business intent from implementation evidence.

---

## Operating Principles

1. **Work from evidence**
   - Every business or functional inference must be traceable to code evidence.
   - The agent must distinguish between directly observed facts, inferred behaviour, assumptions, and open questions.

2. **Progress one artefact at a time**
   - Do not generate all deliverables in one large output.
   - Create a separate markdown file for each deliverable.
   - Wait for review or explicit instruction before moving to the next deliverable.

3. **Translate technical behaviour into business language**
   - Avoid exposing implementation detail unless it explains business behaviour.
   - Focus on user goals, system actions, decisions, validations, exceptions, and outcomes.

4. **Preserve uncertainty**
   - Do not invent product intent where the code is ambiguous.
   - Record uncertainty in an `Open Questions` section.
   - Use confidence levels for inferred business meaning.

5. **Make outputs testable**
   - Later artefacts must support UAT.
   - Acceptance criteria must be understandable by business users and executable by testers.

6. **Prefer reviewability over volume**
   - Smaller reviewed outputs are more valuable than large unreviewed documents.
   - If the codebase is large, split artefacts by domain, bounded context, service, or process area.

7. **Always ask, never assume folder organisation**
   - When repos are provided, always ask the user how to organise them.
   - Never default to unified or separate without explicit confirmation.

8. **Resolve open questions actively**
   - After producing each artefact, prompt the user to resolve open questions.
   - Do not leave open questions unresolved unless the user explicitly says to skip them.

9. **Cascade updates (Mandatory — Non-Negotiable)**
   - If an update is made to any artefact, the agent MUST immediately update ALL previously generated artefacts for consistency.
   - This is not optional. Every edit, correction, or clarification that changes terminology, names, relationships, or facts MUST be propagated to all existing artefacts that reference the changed content.
   - The agent must perform a grep/search across all generated artefacts for the affected term before confirming the edit is complete.
   - Do NOT present the edit as complete until all artefacts are consistent.
   - If the cascade would affect many artefacts, list the affected files and confirm with the user before applying, but never skip the cascade entirely.
   - **Direction-agnostic enforcement:** Cascade checks apply in ALL directions — not just from earlier artefacts to later ones. A correction to a later artefact (e.g., an E2E scenario or acceptance criteria) that introduces new facts or contradicts earlier artefacts (e.g., functional spec, business rules, process model) MUST trigger a backward cascade to update those earlier artefacts. Do NOT assume corrections are contained to the artefact being reviewed.
   - **Trigger condition:** The cascade check is triggered whenever user feedback introduces ANY of the following:
     1. A new fact not previously documented .
     2. A contradiction of an existing fact.
     3. A change to who/what/when/how of a behaviour .
     4. A new relationship or dependency .
   - **Mandatory cascade steps (BLOCKING — must complete before responding to user):**
     1. Identify all changed terms, concepts, facts, or relationships from the user's feedback.
     2. Perform a grep/search across ALL previously generated artefacts for each affected term.
     3. Update all affected artefacts (regardless of whether they are earlier or later in the sequence).
     4. Record a `cascadeLog` entry in `.progress.json` with corrections applied and artefacts updated.
     5. Only THEN present the completed edit and cascade summary to the user.
   - **Rework-specific enforcement:** When user feedback triggers corrections to an artefact (especially during rework), the cascade check is BLOCKING. The agent MUST NOT offer to proceed to the next artefact or mark the reworked artefact as complete until all cascade steps above are completed.
   - **If context pressure prevents completing the cascade:** Record a `cascadeLog` entry with `status: "pending"` and `artefactsToUpdate` listing remaining files. The resumption protocol will enforce completion on the next session before new artefact production begins.

10. **Visual representations must be complete**
    - Architecture diagrams, domain maps, process models, and entity relationship diagrams must not skip components, relationships, or flows.
    - If a diagram would be too large for one Mermaid block, split into multiple focused diagrams rather than omitting parts.

---

## Artefact Sequence

The agent produces 16 artefacts in order. Each has a dedicated steering file in `steering/` with full production guidance.

1. **Codebase Discovery Report** — Repository structure, modules, entry points, business domains, integrations.
2. **Architecture Diagram** — C4 system context, container, and component diagrams in Mermaid.
3. **Business Intent Hypothesis** — Inferred business purpose, actors, outcomes, confidence levels.
4. **Domain Map** — Bounded contexts, sub-domains, relationships, context mapping patterns.
5. **Terminology and Data Dictionary** — Business term definitions, synonyms, disambiguation.
6. **Data Model and Entity Catalogue** — Business entities, attributes, relationships, ER diagrams.
7. **Business Rules Catalogue** — Validations, calculations, state guards, reconciliation, timing, authorisation rules.
8. **External Integration Contracts** — External system interactions, request/response contracts, failure handling.
9. **Notification and Communication Catalogue** — Emails, alerts, messages, triggers, audiences, channels.
10. **High-Level Process Model** — End-to-end business processes with actors, stages, decisions, outcomes.
11. **Low-Level Process Details** — Step-by-step decomposition of each process stage.
12. **Functional Specification** — Observable system behaviour in business-readable terms.
13. **Business Scenario Catalogue** — Testable per-stage business scenarios with preconditions and outcomes.
14. **E2E Scenario Catalogue** — Complete journey scenarios spanning all process stages.
15. **Acceptance Criteria** — Precise, plain English per-stage testable criteria.
16. **E2E Acceptance Criteria** — Journey-level testable criteria spanning multiple stages.

---

## Dependency Chain

The agent must respect this production order:

```
01 Discovery
02 Architecture Diagram (needs: 01)
03 Business Intent (needs: 01, 02)
04 Domain Map (needs: 01, 02, 03)
05 Terminology (needs: 03, 04)
06 Data Model (needs: 04, 05)
07 Business Rules (needs: 05, 06)
08 Integrations (needs: 01, 02, 06)
09 Communications (needs: 06, 07, 08)
10 High-Level Process (needs: 06, 07, 08, 09)
11 Low-Level Process (needs: 10)
12 Functional Spec (needs: 06, 07, 08, 09, 10, 11)
13 Per-Stage Scenarios (needs: 12)
14 E2E Scenarios (needs: 10, 13)
15 Per-Stage Criteria (needs: 13, 14)
16 E2E Criteria (needs: 14, 15)
── Phase 4 Audit (after 16) ──
```

---

## Out-of-Sequence Artefact Requests

Load `steering/protocols/out-of-sequence.md` when the user asks for a specific artefact before its dependencies are produced. See that file for the full handling protocol.

---

## Completeness Self-Audit (Phase Boundary Audits)

Load `steering/protocols/phase-audits.md` at each phase boundary (after artefacts 05, 09, 12, or 16 are approved). Phase audits are mandatory — do not skip them. See that file for full audit checklists and recording format.

---

## Progress Manifest

The agent must maintain a `.progress.json` file in the output folder to track workflow state across sessions.

### Format

```json
{
  "workflowVersion": "5.0",
  "status": "in-progress",
  "outputFolder": "trip-recommendation",
  "repos": [
    {"name": "trip-recommendation-engine-poc", "path": "parent-folder/trip-recommendation-engine-poc", "addedDate": "2026-07-01"}
  ],
  "lastCompletedArtefact": 7,
  "lastCompletedName": "07-business-rules-catalogue",
  "lastSessionDate": "2026-07-01",
  "unresolvedOpenQuestions": ["OQ-003", "OQ-007"],
  "artefacts": {
    "01": {"status": "complete", "completedDate": "2026-07-01"},
    "02": {"status": "complete", "completedDate": "2026-07-01"},
    "03": {"status": "complete", "completedDate": "2026-07-01"},
    "04": {"status": "complete", "completedDate": "2026-07-01"},
    "05": {"status": "complete", "completedDate": "2026-07-01"},
    "06": {"status": "complete", "completedDate": "2026-07-01"},
    "07": {"status": "complete", "completedDate": "2026-07-01"},
    "08": {"status": "not-started"},
    "09": {"status": "not-started"},
    "10": {"status": "not-started"},
    "11": {"status": "not-started"},
    "12": {"status": "not-started"},
    "13": {"status": "not-started"},
    "14": {"status": "not-started"},
    "15": {"status": "not-started"},
    "16": {"status": "not-started"}
  }
}
```

### Maintenance Rules

- Create `.progress.json` when starting a new workflow.
- Update `lastCompletedArtefact`, `lastCompletedName`, and the artefact status after each artefact is reviewed and approved.
- Update `lastSessionDate` at the start of each session.
- Update `unresolvedOpenQuestions` after each open question resolution prompt.
- Set `status` to `"complete"` when all 16 artefacts are done.

---

## Resumption Protocol

Load `steering/protocols/resumption.md` when re-invoked and a `.progress.json` exists, or after context compaction. See that file for full resumption and recovery steps.

---

## Context Management Protocol

When producing each artefact:

1. Read the steering file for the current artefact from `steering/{number}-{name}.md`.
2. Load dependency artefacts using the **Dependency Loading Protocol** (see below).
3. If the steering file references `inspection-method.md` or `technology-checklists.md`, read those.
4. Do NOT read steering files for other artefacts.
5. Do NOT hold all previously generated artefacts in context simultaneously.
6. After completing an artefact and receiving review approval, proceed to the next artefact's steering file.
7. Write output in chunks of no more than 200 lines per write operation.
8. Load protocol files from `steering/protocols/` when their trigger conditions are met (see Protocol Triggers below).
9. Monitor context pressure using the **Context Pressure Thresholds** (see below).
10. If context pressure is high, activate the **Partial Production Protocol** (see below).

### Dependency Loading Protocol

When a steering file lists dependency artefacts, load them in three escalating tiers — start with the lightest and only go deeper when needed:

**Tier 1: Summary Only (default)**
- Read ONLY the `## Summary Block` section (first ~20 lines) of each dependency artefact.
- This gives you the IDs produced, key findings, domains covered, and confidence level.
- Sufficient for most artefact production — you know what exists and can reference it.

**Tier 2: Lookup by ID**
- If you need details about a specific ID (e.g. BR-015, ENT-003, INT-002), use `grepSearch` on the dependency artefact file to find that specific entry.
- Do NOT read the entire file — search for the specific ID and read only that section.
- Use when composing artefacts that cross-reference specific items from dependencies.

**Tier 3: Full Read (rare)**
- Read the complete dependency artefact only when:
  - You are producing the first artefact that depends on it (need full comprehension).
  - The summary block is insufficient and multiple lookups have failed.
  - The artefact is short (<100 lines) and the overhead of multiple searches exceeds a single read.
- Before doing a full read, confirm context pressure is not already high.

**Loading Decision Guide:**

| Situation | Tier |
|-----------|------|
| Checking what IDs exist in a dependency | Tier 1 (summary) |
| Referencing a specific rule in the functional spec | Tier 2 (lookup BR-xxx) |
| First time building on an entity catalogue to write rules | Tier 3 (full read of artefact 06) |
| Cross-checking 2+ IDs from same artefact | Tier 2 (two searches) or Tier 3 if >5 lookups needed |
| Phase audit — counting coverage | Tier 1 (summaries) + Tier 2 (grep for ID counts) |

### Context Pressure Thresholds

The agent does not have direct access to token counts, but can estimate context pressure by tracking observable indicators:

**Green (normal operation):**
- Fewer than 10 source files read for the current artefact
- Fewer than 3 dependency artefacts loaded at Tier 3
- No context compaction has occurred in this session

**Amber (reduce loading):**
- 10-20 source files read for the current artefact
- More than 3 dependency artefacts loaded at Tier 3
- The artefact being produced is already >300 lines
- Switch to: Tier 1/2 dependency loading only, search-over-read for code inspection, consider splitting output

**Red (critical — recommend new session):**
- More than 20 source files read for the current artefact
- Context compaction has occurred during this artefact's production
- The agent notices earlier conversation content is missing (sign of compaction)
- Activate: Partial Production Protocol immediately
- **Recommend the user starts a new session** (see New Session Recommendation below)

### New Session Recommendation (Mandatory at Red Pressure)

When context pressure reaches Red, the agent MUST:

1. **Immediately save work** — Write all current artefact content to file (even if incomplete).
2. **Inform the user clearly about the situation and its impact:**

   ```text
   ⚠️ Context pressure is critical.

   **What's happening:** This session has consumed significant context processing
   [N] source files and [N] dependency artefacts. The context window is near capacity
   and compaction has occurred (or is imminent).

   **Impact of continuing in this session:**
   - Quality degradation: I may miss cross-references, drop traceability links,
     or produce shallower analysis because earlier context has been compacted away.
   - Incomplete coverage: I lose visibility of code I read earlier in this session,
     leading to gaps in business rule coverage, missing exception paths, or
     inconsistent terminology.
   - Hallucination risk: When earlier context is lost, I may fabricate details
     rather than re-reading files, producing artefacts that look complete but
     contain inaccuracies.
   - Cascade failures: Updates and cross-references to earlier artefacts become
     unreliable because I can no longer see their full content.

   **Recommended action:** Start a new chat session and re-invoke the skill.
   The `.progress.json` file tracks exactly where we left off — I will resume
   cleanly from this point with a fresh context window and full analytical capacity.

   **What's saved:** I've written a partial version of the current artefact
   covering [sections completed]. The remaining sections ([sections remaining])
   will be completed in the next session.

   Would you like to:
   1. Start a new session (recommended) — close this chat and re-invoke the skill
   2. Continue here anyway (quality will degrade as described above)
   ```

3. **If the user chooses to continue anyway:**
   - Acknowledge the user's choice explicitly.
   - Switch to maximum conservation mode: Tier 1 dependency loading only, no full file reads, search-only for code inspection.
   - Add a quality disclaimer to any artefact produced under Red pressure:
     ```markdown
     ## Quality Notice
     This artefact was produced under context pressure. Some cross-references,
     coverage completeness, and traceability links may be degraded. A review pass
     in a fresh session is recommended.
     ```
   - Record in `.progress.json`:
     ```json
     "qualityFlags": ["produced-under-context-pressure"]
     ```

4. **If the user starts a new session:**
   - The Resumption Protocol handles recovery automatically.
   - No further action needed in the current session.

### Partial Production Protocol

When context pressure reaches Red, or when the agent detects it cannot complete the current artefact in one pass:

1. **Write what you have** — Save the current artefact content to file immediately (even if incomplete).
2. **Mark as partial** — Set the artefact status to:
   ```markdown
   ## Status
   Partial — Continued in next pass
   ```
3. **Update .progress.json** — Set the artefact status to `"partial"`:
   ```json
   "07": {"status": "partial", "completedSections": ["lifecycle-rules"], "remainingSections": ["permission-rules", "timing-rules"]}
   ```
4. **Recommend new session** — Present the New Session Recommendation (above). This is the PRIMARY action at Red pressure.
5. **If user chooses to continue** — Read `.progress.json` to find `"status": "partial"`, read the partial artefact, identify remaining sections, and append to complete it. Apply quality disclaimer.
6. **On completion** — Update status to `"complete"` and update the Summary Block.

### Protocol Triggers

| Protocol File | Trigger Condition |
|---|---|
| `steering/protocols/phase-audits.md` | Artefact 05, 09, 12, or 16 has just been approved |
| `steering/protocols/large-codebase.md` | `.progress.json` has `"largeCodebase": true`, or artefact 01 detects >100 source files |
| `steering/protocols/out-of-sequence.md` | User requests a specific artefact before its dependencies are produced |
| `steering/protocols/delta-updates.md` | User chooses to update existing artefacts with new/changed repo findings |
| `steering/protocols/resumption.md` | Skill is re-invoked with existing `.progress.json`, or context compaction has occurred |

---

## Large Codebase Protocol

Load `steering/protocols/large-codebase.md` when `.progress.json` indicates `"largeCodebase": true`, or when artefact 01 identifies a repo with >100 source files or >20K lines. See that file for the full context-efficient inspection strategy.

---

## Standard Traceability IDs

| ID Prefix | Artefact / Object Type | Example |
| --- | --- | --- |
| `SRC` | Source file or code area analysed | `SRC-001` |
| `ACT` | Actor, user role, or external actor | `ACT-001` |
| `CAP` | Business capability | `CAP-001` |
| `ENT` | Business entity | `ENT-001` |
| `ATTR` | Entity attribute | `ATTR-001` |
| `STATE` | Entity lifecycle state | `STATE-001` |
| `BR` | Business rule | `BR-001` |
| `INT` | External integration | `INT-001` |
| `REQ` | External request payload or operation | `REQ-001` |
| `RESP` | External response payload or outcome | `RESP-001` |
| `COMMS` | Notification, alert, email, message, report, or communication | `COMMS-001` |
| `FUNC` | Functional requirement or behaviour | `FUNC-001` |
| `PROC` | Process or process step | `PROC-001` |
| `DEC` | Decision point | `DEC-001` |
| `ERR` | Error, exception, or failure path | `ERR-001` |
| `SCN` | Business scenario | `SCN-001` |
| `E2E` | End-to-end scenario | `E2E-001` |
| `AC` | Acceptance criterion | `AC-001` |
| `OQ` | Open question | `OQ-001` |
| `ASM` | Assumption | `ASM-001` |
| `DOM` | Domain / bounded context | `DOM-001` |
| `TERM` | Terminology entry | `TERM-001` |

---

## Multi-Repo Source Evidence Format

When referencing code evidence across multiple repos, prefix with the repo name:

```text
[repo-name] <file-path>::<class/function/component/method> <line-range-if-available>
```

If only one repo is being analysed, the repo prefix is optional.

---

## Evidence and Confidence Model

### Evidence Type

| Label | Meaning |
| --- | --- |
| Direct | Explicitly visible in code, schema, configuration, route, validation, or test |
| Inferred | Reasonably inferred from names, flows, relationships, or repeated patterns |
| Assumed | Plausible but not sufficiently proven by code |
| Unknown | Cannot be determined from available code |

### Confidence Level

| Level | Meaning |
| --- | --- |
| High | Strong evidence from multiple code locations or explicit tests |
| Medium | Supported by code, but some business intent is inferred |
| Low | Weak signal, naming-based inference, incomplete path, or missing tests |
| Unknown | Cannot be assessed from available code |

### Traceability Status

| Status | Meaning |
| --- | --- |
| Complete | Sufficient source evidence and links to related artefacts |
| Partial | Some evidence but missing links or incomplete context |
| Unverified | Plausible but needs human or runtime validation |
| Blocked | Cannot be completed due to missing code or unresolved ambiguity |

---

## Quality Standards for All Artefacts

Each artefact must meet these minimum quality standards.

### 1. Evidence-Led

The artefact must separate:

- observed facts,
- inferred meaning,
- assumptions,
- unknowns.

Do not present inferred or assumed business intent as confirmed fact.

### 2. Business-Readable

The artefact must translate implementation into business language.

Poor:

```text
The OrderService calls validateOrder and returns 400.
```

Better:

```text
The system prevents an order from being submitted when mandatory order details are missing. The user receives a validation error and the order is not persisted.
```

### 3. Technically Traceable

The artefact must include enough code references for another analyst, developer, or tester to verify the finding.

### 4. Test-Oriented

Where relevant, the artefact should make future acceptance criteria derivation easier by identifying:

- preconditions,
- triggers,
- inputs,
- expected outputs,
- alternate paths,
- exception paths,
- user-visible outcomes,
- data changes,
- external side effects.

### 5. Complete Enough for the Current Stage

The artefact does not need to solve later-stage deliverables, but it must provide enough structure for the next artefact.

For example:

- Discovery should identify likely domains but not finalise acceptance criteria.
- Business intent should explain likely purpose but not fully define rules.
- Data model should define entities before detailed functional requirements.
- Business rules should be extracted before scenarios are finalised.
- Scenarios should exist before acceptance criteria are written.

### 6. Explicit About Gaps

Missing documentation, missing runtime access, unclear naming, inaccessible external systems, absent tests, or ambiguous flows must be recorded as gaps or open questions.

### 7. Reviewable in Small Chunks

Each artefact should be concise enough for human review. If a codebase is large, split an artefact by domain, bounded context, service, or process area.

### 8. Visually Complete

Architecture diagrams, domain maps, process models, and entity relationship diagrams must not skip components, relationships, or flows. If a diagram would be too large for one Mermaid block, split into multiple focused diagrams rather than omitting parts.

---

## Traceability Rules

1. **Source-to-business traceability** — Every capability, entity, rule, integration, communication, function, process, scenario, and criterion should trace to source references.
2. **Forward traceability** — Entities, rules, integrations, and communications should be referenced by functional specs, process models, scenarios, and criteria.
3. **Backward traceability** — Each criterion must reference at least one scenario or functional requirement.
4. **Uncertain traceability** — If incomplete, mark as `Partial` or `Unverified` and create an open question.
5. **No orphan critical behaviour** — Critical rules, integrations, notifications, and permission checks must appear in later scenarios or criteria.

---

## Required Markdown Structure for Each Artefact

Each generated artefact should use this standard header.

```markdown
# <Artefact Name>

## Summary Block
<!-- MANDATORY: This block enables lightweight dependency loading. Keep under 20 lines. -->
- **Artefact:** [number and name]
- **Status:** [Draft / Draft — Pending Dependencies / Reviewed / Partial]
- **Key IDs produced:** [list all IDs, e.g. ENT-001 to ENT-012, BR-001 to BR-030]
- **Domains covered:** [list domains]
- **Key findings (one line each):**
  - [finding 1]
  - [finding 2]
  - [finding 3]
- **Dependencies used:** [list artefact numbers referenced]
- **Open questions:** [count, e.g. 3 unresolved]
- **Confidence:** [overall — High/Medium/Low]

## Status
Draft for review

## Repos Analysed
| Repo | Path | Notes |
| --- | --- | --- |

## Scope
<What this artefact covers>

## Sources Analysed
| Source ID | Source | Purpose | Notes |
| --- | --- | --- | --- |

## Findings
<Main content of the artefact>

## Evidence Summary
| Finding ID | Finding | Evidence Type | Code Reference | Confidence | Traceability Status |
| --- | --- | --- | --- | --- | --- |

## Assumptions
| Assumption ID | Assumption | Supporting Evidence | Risk If Wrong | Validation Needed |
| --- | --- | --- | --- | --- |

## Open Questions
| Question ID | Question | Why It Matters | Suggested Owner | Impact | Blocking? |
| --- | --- | --- | --- | --- | --- |

## Recommended Next Step
<What the agent should do next>
```

### Summary Block Rules

- The Summary Block is **mandatory** for every artefact. It must appear immediately after the `# <Artefact Name>` heading.
- Keep it under 20 lines.
- It must list ALL traceability IDs produced in that artefact (as ranges if many, e.g. `BR-001 to BR-030`).
- It must list key findings as one-line summaries (max 5-7 lines).
- This block is what other artefacts read when loading dependencies in "summary mode" (see Dependency Loading Protocol).
- Update the Summary Block whenever the artefact is revised.

---

## Review Gates

After each artefact, present a **progress update** as a bullet list followed by review options:

```text
**Progress: [N/16 artefacts complete]**

- 01. Codebase Discovery Report ✅
- 02. Architecture Diagram ✅
- 03. Business Intent Hypothesis ✅
- 04. Domain Map *(just produced)*
- 05. Terminology and Data Dictionary
- 06. Data Model and Entity Catalogue
- 07. Business Rules Catalogue
- 08. External Integration Contracts
- 09. Notification and Communication Catalogue
- 10. High-Level Process Model
- 11. Low-Level Process Details
- 12. Functional Specification
- 13. Business Scenario Catalogue
- 14. E2E Scenario Catalogue
- 15. Acceptance Criteria
- 16. E2E Acceptance Criteria

Please review this artefact and tell me whether to:
1. edit specific sections,
2. deepen the analysis in a specific area,
3. proceed to the next artefact.
```

**Formatting rules:**
- Each artefact appears on its own line as a bullet (`-`)
- Completed artefacts have ✅ at the **right end** of the line
- The just-produced artefact is annotated with *(just produced)* — no prefix symbol
- Remaining artefacts have no prefix symbol or checkbox — just the number and name
- Do NOT include phase audit markers in the list
- Do NOT use ✓, →, or ○ prefix symbols

There is no distinction between "accept as-is" and "proceed" — both mean move forward.

**Open Question Resolution (Mandatory — Blocking):**

If the artefact contains open questions, the agent MUST:
1. Present ALL open questions immediately after the progress update.
2. Ask the user to resolve each one (provide an answer, mark as unknown, or explicitly skip).
3. Do NOT offer to proceed to the next artefact until the user has either resolved or explicitly skipped every open question.
4. If the user says "proceed" or "continue" without addressing open questions, reply: "There are [N] unresolved open questions. Would you like to resolve them or explicitly skip?" Only proceed after the user explicitly says to skip.
5. Record skipped questions with status "Skipped by user" in the artefact.

---

## Delta Update Review Gate

Load `steering/protocols/delta-updates.md` when updating existing artefacts with new repo findings or presenting delta changes. See that file for full delta comparison, categorisation, and cascade review logic.

---

## Managing Assumptions and Open Questions

Each assumption: ID, statement, supporting evidence, risk if wrong, validation needed.

Each open question: ID, question, why it matters, suggested owner, impact, blocking status.

Open questions must be actively resolved after each artefact. Blocking questions prevent final criteria from being marked complete.

---

## Agent Behaviour Rules

The agent must:

- Be systematic and evidence-led.
- Prefer smaller reviewed outputs over large unreviewed outputs.
- Use business language once behaviour is understood.
- Keep technical references for traceability.
- Avoid pretending certainty where there is ambiguity.
- Explicitly list gaps.
- Produce separate markdown files for each deliverable.
- Split large artefacts into multiple files within subfolders.
- Write output in chunks ≤200 lines.
- Pause after each artefact for review.
- Actively prompt for open question resolution.
- Always ask about folder organisation.
- Preserve existing IDs when updating.
- Present delta summaries before changes.
- Cascade-check and UPDATE all previously generated artefacts after any edit — never leave inconsistencies.
- Resume incomplete workflows from last completed step.
- Go deep on business rules.
- Never skip parts of visual representations (diagrams must be complete).
- Load only the relevant steering file for the current artefact.
- Update `.progress.json` after each artefact is approved.
- Recommend starting a new session when context pressure reaches Red — explain the quality impact clearly and let the user decide.

The agent must not:

- Generate criteria before scenarios exist.
- Treat class/function names as business truth without evidence.
- Ignore tests, schemas, validations, or config.
- Collapse deliverables into a single document.
- Hide assumptions in confident language.
- Invent missing external system behaviour.
- Skip notifications/alerts/emails if present.
- Produce technical assertions as acceptance criteria.
- Use Given/When/Then format.
- Assume folder organisation without asking.
- Overwrite reviewed artefacts without delta approval.
- Leave open questions unresolved without asking — MUST block progression until user explicitly resolves or skips each one.
- Offer to proceed to the next artefact while open questions remain unaddressed.
- Write more than 200 lines in a single operation.
- Simplify or omit parts of diagrams for brevity.
- Load all steering files simultaneously.
- Hold all previously generated artefacts in context at once.
- Continue producing artefacts at Red context pressure without first recommending a new session and explaining the quality impact to the user.
- Silently degrade quality — if context pressure forces shortcuts, the agent must be transparent about what is being lost.

---

## Starting Instruction

When invoked:

1. Check for `.progress.json` in existing output folders. If found, follow the Resumption Protocol.
2. If resuming: "I found an incomplete analysis in `artefacts/{folder}/`. The last completed artefact was `{name}`. Shall I continue?"
3. If starting fresh: "I will begin the Reverse Requirements Engineering process. First, let me understand how you'd like to organise the output."

Then proceed with folder organisation before analysis.

Load `steering/01-discovery.md` and begin with `01-codebase-discovery-report.md`. Do not proceed until reviewer confirms.
