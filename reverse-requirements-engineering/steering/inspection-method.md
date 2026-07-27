---
inclusion: manual
---
# Codebase Inspection Method — Steering Guide

## Purpose

This is the full 12-stage codebase inspection method. Load this file when a per-artefact steering file references it for code inspection guidance.

---

## Stage 1: Repository Triage

**Goal:** Understand what kind of system this is and how it is organised.

Inspect:

- Repository root files
  - `README`, `CONTRIBUTING`, `CHANGELOG`, `LICENSE`
  - package or build files such as `package.json`, `pom.xml`, `build.gradle`, `.csproj`, `requirements.txt`, `pyproject.toml`, `go.mod`, `Cargo.toml`
  - container and deployment files such as `Dockerfile`, `docker-compose.yml`, Kubernetes manifests, Helm charts
  - CI/CD files such as GitHub Actions, Azure Pipelines, Jenkins, GitLab CI
  - environment and configuration examples such as `.env.example`, `application.yml`, `appsettings.json`, config maps

Determine:

- primary language and framework
- application type
- likely runtime entry point
- deployable units
- major dependencies
- test frameworks
- database or persistence technology
- integration technology
- whether the codebase appears to contain frontend, backend, infrastructure, scripts, batch jobs, or multiple services

Capture:

- initial repository map
- likely business-facing modules
- technical-only modules to deprioritise unless they affect behaviour
- missing documentation or missing runtime context

---

## Stage 2: Architecture and Boundary Mapping

**Goal:** Identify system boundaries, internal components, and external touchpoints.

Inspect:

- application bootstrap files
- routing configuration
- module registration
- dependency injection setup
- service registration
- API gateway or controller definitions
- queue, event, or scheduler configuration
- database connection configuration
- external client configuration
- authentication and authorisation setup

Determine:

- inbound interfaces:
  - REST APIs
  - GraphQL APIs
  - UI routes
  - CLI commands
  - batch job triggers
  - scheduled jobs
  - queue/event consumers
  - webhooks
  - file drops or imports
- outbound interfaces:
  - external APIs
  - queues or topics
  - emails or notifications
  - file exports
  - database writes
  - reporting outputs
- internal layers:
  - presentation or API layer
  - application/use-case layer
  - domain/business logic layer
  - data access layer
  - integration/client layer
  - infrastructure layer

Capture:

- component map
- inbound and outbound interaction list
- suspected business workflows
- areas requiring deeper analysis

---

## Stage 3: Business Domain Discovery

**Goal:** Identify business concepts, entities, capabilities, and user actions.

Inspect names and behaviour in:

- routes, controllers, pages, handlers, resolvers
- service, use-case, command, query, and workflow classes
- domain models, entities, aggregates, value objects
- database schemas, migrations, seed data
- validation schemas
- event names and payloads
- message templates
- test names and fixtures
- feature flags and configuration keys

Look for:

- nouns that represent business entities
- verbs that represent business actions
- status fields and lifecycle transitions
- IDs and relationships
- date, amount, quantity, eligibility, ownership, approval, submission, cancellation, expiry, or business domain concepts
- user roles, permissions, and actor names
- repeated concepts across API, domain, data, and tests

Capture:

- candidate business entities
- candidate capabilities
- candidate business events
- candidate lifecycle states
- candidate actors and roles
- glossary terms inferred from code

Important:

- Names alone are not enough. Treat names as leads, then validate them through flow, data, tests, and rules.

---

## Stage 4: Entry Point to Outcome Flow Tracing

**Goal:** Trace behaviour from trigger to business outcome.

For each major entry point, trace the full execution path:

1. What triggers the behaviour?
2. What input is accepted?
3. What validation occurs?
4. What user, role, tenant, or context is required?
5. What service/use-case is called?
6. What business rules are evaluated?
7. What data is read?
8. What data is created, updated, deleted, or published?
9. What external systems are called?
10. What notifications, alerts, emails, events, reports, or files are generated?
11. What response or result is returned?
12. What errors, exceptions, retries, fallbacks, or alternate paths exist?

Capture:

- trigger
- preconditions
- inputs
- rules
- decisions
- data changes
- outputs
- side effects
- error paths
- business interpretation

This flow tracing is the core technique for converting code into functional specifications, scenarios, and acceptance criteria.

---

## Stage 5: Data and State Analysis

**Goal:** Understand what the system stores, how entities relate, and how state changes over time.

Inspect:

- database migrations
- ORM models
- entity classes
- schema definitions
- repositories and queries
- data transfer objects
- event payloads
- fixtures and seed data
- test data builders
- reporting queries

Determine:

- entity names and business meanings
- key fields and identifiers
- required and optional fields
- relationships and cardinality
- status/state fields
- state transition rules
- audit fields
- soft delete or archival behaviour
- data retention hints
- calculated or derived fields
- default values
- uniqueness constraints
- referential integrity rules

Capture:

- entity catalogue
- entity relationship summary
- lifecycle states
- data quality rules
- evidence-backed field meanings

---

## Stage 6: Business Rule Extraction

**Goal:** Extract rules that constrain or drive business behaviour.

**Go deep.** Do not stop at surface-level validations. Trace into:

Inspect:

- validation logic — field-level, cross-field, cross-entity, and contextual validations
- reconciliation logic — matching, balancing
- state transition guards — what must be true before a state change is allowed
- calculation logic — fees, interest, apportionment, derived amounts
- timing constraints — cut-offs, deadlines, scheduling rules, expiry logic
- conditional branches in services/use cases
- policy or specification classes
- rules engines
- database constraints
- configuration thresholds
- feature flags
- authorisation checks
- tests, especially edge cases and negative cases
- error messages
- status transition guards

Look for:

- eligibility rules
- mandatory field rules
- amount, date, quantity, and threshold rules
- uniqueness and duplication rules
- role/permission rules
- lifecycle transition rules
- cut-off, expiry, timeout, and scheduling rules
- exception and override rules
- integration failure rules
- notification trigger rules
- reconciliation and matching rules


Capture each rule with:

- business description
- source code reference
- trigger/context
- input data
- condition
- pass outcome
- fail outcome
- confidence
- open questions

---

## Stage 7: Integration Analysis

**Goal:** Understand external dependencies through actual code behaviour.

Inspect:

- HTTP clients
- SDK clients
- queue producers/consumers
- event publishers/subscribers
- webhook handlers
- file import/export logic
- authentication clients
- retry and resilience configuration
- integration tests and mocks
- contract tests
- environment variables for endpoints and credentials

For each integration, capture:

- external system name, if visible
- purpose inferred from the calling flow
- direction: inbound or outbound
- trigger
- request method and endpoint/path where visible
- request headers and authentication pattern where visible
- request payload structure
- response payload structure
- success and failure handling
- retry, timeout, fallback, and circuit-breaker behaviour
- mapping to internal business entities
- business impact if unavailable or failed

Special instruction:

When external documentation is absent, document only the observable request/response contract and business interpretation supported by code. Do not invent undocumented external behaviour.

---

## Stage 8: Notification and Communication Analysis

**Goal:** Identify user-facing and system-facing communications that form part of the business or UX journey.

Inspect:

- email templates
- notification services
- alerting code
- UI message components
- validation messages
- error messages
- audit event publishers
- reporting/export jobs
- SMS or push notification clients
- webhook/event messages sent to downstream consumers
- scheduled reminder or escalation jobs

For each communication, capture:

- trigger
- audience or recipient
- channel
- subject/title/body/template
- variables used
- timing
- suppression or eligibility rules
- expected user action
- business purpose
- failure handling
- related process step

---

## Stage 9: Test Suite Mining

**Goal:** Use tests as behavioural evidence, not just technical validation.

Inspect:

- unit tests
- integration tests
- end-to-end tests
- contract tests
- snapshot tests
- fixtures
- mocks and stubs
- test data builders

Extract:

- expected behaviours
- business rules
- negative cases
- edge cases
- role and permission cases
- integration assumptions
- notification expectations
- state transition expectations
- terminology used by previous developers

Important:

- Tests often reveal business intent more clearly than production code.
- However, tests may be stale or incomplete. Mark confidence accordingly.

---

## Stage 10: User Interface and UX Journey Analysis

Use this stage when the codebase contains a frontend, web app, mobile app, or UI layer.

Inspect:

- route definitions
- pages/screens
- forms
- buttons and user actions
- labels and helper text
- validation messages
- error states
- loading states
- empty states
- success states
- navigation rules
- role-based UI behaviour
- API calls made by the UI
- analytics events, if present

Capture:

- user journeys
- user goals
- screen-level capabilities
- form fields and validations
- visible statuses and messages
- UX-triggered notifications or alerts
- frontend/backend behaviour alignment gaps

---

## Stage 11: Security, Roles, and Permissions Analysis

**Goal:** Understand who can do what, and under what conditions.

Inspect:

- authentication middleware
- authorisation policies
- role checks
- permission constants
- claims/scopes
- tenant or organisation isolation logic
- ownership checks
- admin-only functions
- security-related tests

Capture:

- actors and roles
- permissions per feature or action
- data visibility rules
- ownership or tenancy restrictions
- unauthorised and forbidden behaviours
- audit implications

---

## Stage 12: Operational Behaviour Analysis

**Goal:** Identify behaviours that affect business outcomes but may not be visible in normal user flows.

Inspect:

- scheduled jobs
- batch processing
- retries
- dead-letter queues
- cleanup/archive jobs
- reconciliation jobs
- monitoring and alerting hooks
- feature flags
- configuration-driven behaviour
- migrations and data repair scripts

Capture:

- operational triggers
- business processes executed in the background
- timing and frequency
- failure handling
- manual intervention points
- downstream impacts

---

## Standard Analysis Workflow

For each deliverable, the agent must follow this workflow.

### Step 1: Inspect relevant code systematically

Build a map of the repository, then inspect code in layers from entry points to business behaviour. Use the stages above as appropriate for the artefact being produced.

### Step 2: Extract evidence

Create a temporary internal evidence map: file path, function/class/module name, observed behaviour, business interpretation, evidence type, confidence level, questions raised.

### Step 3: Produce the markdown artefact

Write in chunks of no more than 200 lines per write operation. Include: purpose, scope, findings, evidence references, assumptions, open questions, recommended next step.

### Step 4: Ask for review and resolve open questions

After producing each artefact, stop and ask:

```text
Please review this artefact and tell me whether to:
1. edit specific sections,
2. deepen the analysis in a specific area,
3. proceed to the next artefact.
```

Then immediately prompt to resolve open questions:

```text
This artefact has the following open questions that need resolution:
- OQ-001: [question]
- OQ-002: [question]

Can you help resolve these, or should I skip them for now?
```

Do not leave open questions unresolved unless the user explicitly says to skip them.
