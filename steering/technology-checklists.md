---
inclusion: manual
---
# Technology-Specific Inspection Checklists — Steering Guide

## Purpose

Use the relevant checklist based on the codebase technology. Load this file when a per-artefact steering file references it for technology-specific inspection guidance.

---

## Backend API or Service

Inspect:

- routes/controllers/handlers/resolvers
- request and response models
- service/use-case classes
- validators
- domain models
- repositories
- database migrations
- middleware
- authentication and authorisation
- external clients
- tests

Prioritise tracing:

```text
API request -> validation -> business service -> data change -> integration/notification -> response
```

---

## Frontend Web or Mobile App

Inspect:

- routes/pages/screens
- forms and validation
- API client calls
- state management
- role-based rendering
- feature flags
- user messages
- analytics events
- tests and storybook stories, if present

Prioritise tracing:

```text
user action -> UI validation -> API call -> success/error state -> user-visible outcome
```

---

## Event-Driven Service

Inspect:

- consumers/subscribers
- producers/publishers
- event schemas
- queue/topic names
- retry and dead-letter handling
- idempotency logic
- correlation IDs
- integration tests

Prioritise tracing:

```text
event received -> validation/idempotency -> business rule -> state change -> event emitted/notification sent
```

---

## Batch or Scheduled Job

Inspect:

- scheduler configuration
- job entry points
- queries selecting work
- processing rules
- output generation
- retry/failure handling
- reporting or reconciliation logic
- operational alerts

Prioritise tracing:

```text
schedule/manual trigger -> records selected -> rules applied -> updates/outputs generated -> failures reported
```

---

## Database-Heavy Application

Inspect:

- schema migrations
- stored procedures
- triggers
- views
- constraints
- reporting queries
- ORM mappings
- seed data

Prioritise tracing:

```text
data entity -> constraints/rules -> state transitions -> downstream usage
```

---

## Infrastructure-as-Code Repository

Inspect:

- environment definitions
- deployed services
- queues/topics
- databases
- secrets references
- network boundaries
- scheduled triggers
- permissions/IAM
- monitoring/alerts

Prioritise tracing:

```text
deployed component -> runtime dependency -> business impact or operational constraint
```

---

## Monorepo or Multi-Service System

Inspect:

- workspace/package configuration
- service boundaries
- shared libraries
- API contracts between services
- event contracts
- common domain models
- deployment topology
- cross-service tests

Prioritise tracing:

```text
business capability -> participating services -> data/event/API handoffs -> final business outcome
```
