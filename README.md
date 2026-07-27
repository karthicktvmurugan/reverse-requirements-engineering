# 🔄 Reverse Requirements Engineering Skill

> **BLUF:** An evidence-led, 16-step AI agent skill that systematically analyzes un-documented or legacy codebases to reverse-engineer business intent, domain maps, process workflows, and plain-English UAT acceptance criteria with 100% code traceability.

[![Developed & Tested in Kiro](https://img.shields.io/badge/Tested%20in-Kiro-orange)](#-agent-compatibility--installation)
[![Agent Agnostic](https://img.shields.io/badge/Compatibility-Agent--Agnostic-blue)](#-agent-compatibility--installation)
[![Skill Version](https://img.shields.io/badge/Version-5.0-green)](#)

---

## 💡 Overview

When software teams inherit legacy systems, perform modernisations, or onboard new engineers, documentation is often outdated or missing entirely. 

Originally **developed and tested within Kiro**, this skill provides an open, agent-agnostic operational protocol for AI coding agents. Instead of generating surface-level code summaries, the agent acts as a **Technical Lead + Business Analyst pair**, extracting grounded business rules, domain maps, and testable specifications directly from source code and tests.

While optimized for Kiro's spec-driven workflow, this skill relies strictly on universal tool interfaces (`readFile`, `writeFile`, `listDirectory`, `grepSearch`), making it **100% compatible with any AI coding agent**.

---

## ⚡ Key Highlights & Core Principles

| Feature | Description |
| :--- | :--- |
| **Strict Evidence Grounding** | Every rule, entity, and process step links directly to code references (`SRC-xxx`, `BR-xxx`, `ENT-xxx`). No unbacked assumptions. |
| **Iterative Human Review** | Generates one deliverable at a time, pausing for review. Progression is blocked until open questions (`OQ-xxx`) are resolved or skipped. |
| **Direction-Agnostic Cascading** | When human review introduces corrections or new facts, a background cascade engine searches and automatically updates all prior artefacts. |
| **Plain-English Criteria** | Generates business-readable UAT criteria (non-Gherkin) directly executable by QA and product owners. |
| **Context Window Defense** | Uses a 3-tier dependency loading model and context pressure monitoring to prevent AI memory saturation and hallucinations. |

---

## 🚀 Key Use Cases

1. **New Team Onboarding:** Accelerate understanding of complex codebases from weeks to hours with structured domain maps and data dictionaries.
2. **Legacy Modernisation:** Extract a clear "as-is" reference (business rules, state transitions, validation logic) prior to rebuilding or migrating code.
3. **UAT & Test Strategy:** Generate testable per-stage scenarios and cross-stage E2E acceptance criteria straight from implementation logic.
4. **Vendor / Team Handovers:** Establish a shared vocabulary and verifiable knowledge base with complete code-to-requirement traceability IDs.
5. **Feature Blast-Radius Assessment:** Evaluate proposed code changes against documented entity relationships and business rule dependencies.

---

## 📦 Pipeline Structure (16 Artefacts)

The skill guides the agent through a 4-phase sequential workflow:

Phase 1: Discovery & Foundation
* 01-codebase-discovery-report.md
* 02-architecture-diagram.md
* 03-business-intent-hypothesis.md
* 04-domain-map.md
* 05-terminology-and-data-dictionary.md

Phase 2: Evidence Extraction
* 06-data-model-and-business-entity-catalogue.md
* 07-business-rules-catalogue/
* 08-external-integration-contract-summary.md
* 09-notification-alert-and-communication-catalogue.md

Phase 3: Process & Specification
* 10-high-level-process-model/
* 11-low-level-process-details/
* 12-functional-specification/

Phase 4: Scenarios & Acceptance Criteria
* 13-business-scenario-catalogue/
* 14-e2e-scenario-catalogue/
* 15-acceptance-criteria/
* 16-e2e-acceptance-criteria/

---

## 🤖 Agent Compatibility & Installation

Though built and validated in Kiro, this skill adheres to open agent skill standards and works seamlessly across various AI agent platforms.

### 1. Kiro (Native / Primary Target)

Place the skill files into your project or global Kiro skills folder:

```bash
# Project-Specific
mkdir -p .kiro/skills/reverse-requirements-engineering
cp -r * .kiro/skills/reverse-requirements-engineering/

# Global Scope
mkdir -p ~/.kiro/skills/reverse-requirements-engineering
cp -r * ~/.kiro/skills/reverse-requirements-engineering/
```

Run via Kiro agent prompt or command: /reverse-requirements-engineering or "Run the reverse requirements engineering skill on this repo."

### 2. Claude Code/ Cursor/ other agents
Place the skill files (skill.md and steering folder) in your project or global skills folder, and the run by invoking the skill name.

## 🛠️ How It Works (Agent Execution)
**Workspace Setup:** The agent checks .progress.json or asks how you wish to organize the output (artefacts/{folder-name}/).

**Iterative Analysis:** The agent inspects code using stack-specific checklists (APIs, Event-Driven, Batch Jobs, DBs) and generates one artefact at a time.

**Review Gate:** The agent presents progress, lists unresolved open questions (OQ-xxx), and waits for user confirmation (proceed, edit, or deepen).

**Cascading Sync:** Edits trigger an automated grep search across all existing files to update terminology, relationships, and rule IDs backward and forward.

**State Resumption:** Workflow progress is tracked in .progress.json, allowing multi-session restarts without losing place or context.
