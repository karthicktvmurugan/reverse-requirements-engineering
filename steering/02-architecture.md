---
inclusion: manual
---
# Architecture Diagram — Steering Guide

## Dependencies
Read these artefacts before producing this one:
- 01 Codebase Discovery Report

## Load These References
- `steering/inspection-method.md` (Stage 2: Architecture and Boundary Mapping)
- `steering/technology-checklists.md` (select relevant technology type)

---

## What to Produce

**File:** `02-architecture-diagram.md`

**Purpose:** Provide a visual system context and component view of the system(s), translating the discovery report into a structural representation.

**Must include (do not skip any part):**

**System Context Diagram (C4 Level 1):**

- The system under analysis as the central element
- All external actors (users, roles, external systems, upstream/downstream consumers)
- All external system interactions (APIs, queues, databases, file transfers)
- Direction of data flow between actors and the system
- Rendered as a Mermaid diagram

**Container Diagram (C4 Level 2):**

- All deployable units / services within the system boundary
- Databases, message queues, caches, and file stores
- Communication protocols between containers (HTTP, gRPC, events, queues)
- External systems at the boundary
- Rendered as a Mermaid diagram

**Component Interactions (if the system has internal modules/layers):**

- Key internal components or modules within each container
- Dependencies between components
- Which components own which business capabilities
- Rendered as a Mermaid diagram

**Guidelines for visual completeness:**

- Every service, database, queue, external system, and actor discovered in artefact 01 must appear in at least one diagram.
- If the system is too large for a single diagram, produce multiple focused diagrams (e.g. one per bounded context or service group) rather than omitting elements.
- Label all connections with protocol/mechanism (REST, event, queue, DB read/write).
- Include a legend if the diagram uses custom notation.

**Should also include:**

- Narrative description of the architecture
- Key architectural decisions observable from code (e.g. event-driven, CQRS, microservices)
- Technology choices per component
- Deployment topology if observable (Docker, Kubernetes, serverless)
- Code evidence references

---

## Definition of Done

Done when:

- system context diagram (C4 Level 1) is produced showing all actors and external systems,
- container diagram (C4 Level 2) is produced showing all services, databases, and queues,
- all discovered components from artefact 01 appear in at least one diagram,
- all connections are labelled with protocol/mechanism,
- narrative description of the architecture is included,
- key architectural decisions observable from code are documented,
- technology choices per component are listed.

**Do not proceed if:**

- known services or databases are missing from diagrams,
- external systems discovered in artefact 01 are not shown,
- diagrams are incomplete or simplified by omitting elements,
- connections are unlabelled.
