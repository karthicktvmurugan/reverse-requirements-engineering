---
inclusion: manual
---
# External Integration Contract — Steering Guide

## Dependencies
Read these artefacts before producing this one:
- 01 Codebase Discovery Report
- 02 Architecture Diagram
- 06 Data Model and Entity Catalogue

## Load These References
- `steering/inspection-method.md` (Stage 7: Integration Analysis)
- `steering/technology-checklists.md` (select relevant technology type)

---

## What to Produce

**File:** `08-external-integration-contract-summary.md`

**Purpose:** Document any external system interactions discovered in the codebase(s), even when no external documentation exists.

**Should include:**

- External system or endpoint name, if observable
- Integration purpose inferred from code
- Direction of interaction: outbound, inbound, webhook, event, queue, file transfer, or unknown
- Triggering process or code path
- Request method, URL/path, headers, parameters, and payload where observable
- Response structure, status codes, success signals, and error signals where observable
- Retry, timeout, fallback, circuit-breaker, or failure-handling behaviour
- Authentication or authorisation mechanism where observable
- Mapping between internal entities and external request/response fields
- Business impact of successful and failed integration calls
- Evidence references
- Open questions

**Special rule:**

If an external integration is encountered but no external reference documentation is available in the codebase, the agent must focus on what the code sends and receives. The agent must not invent the external system's full business purpose beyond what the code supports.

---

## Definition of Done

Done when:

- each integration has an `INT` ID,
- observable requests have `REQ` IDs,
- observable responses have `RESP` IDs,
- request and response structures are described,
- success and failure handling are documented,
- retry, timeout, fallback, and authentication behaviour are captured where observable,
- business impact of failure is described.

**Do not proceed if:**

- the agent invents external documentation that is not present,
- integrations are named without documenting actual code behaviour,
- failures and response handling are ignored.
