# Large Codebase Protocol

## When to Load
Load this file when `.progress.json` indicates `"largeCodebase": true`, or when artefact 01 (Discovery) identifies a repository with more than ~100 source files or 20,000 lines of code, or when multiple repositories are being analysed together.

---

## Large Codebase Protocol

When the repository under analysis contains more than approximately 100 files or 20,000 lines of code, the agent must apply the following context-efficient inspection strategy. Do not attempt to read all files — work systematically from the outside in.

### Principle 1: Triage First, Deep-Dive Selectively

1. **Map the territory** — Use `listDirectory` with depth to understand folder structure. This consumes minimal context.
2. **Read configuration and entry points only** — Build files, deployment configs, routing, DI setup. These reveal architecture without reading business logic.
3. **Identify the 20% of files that contain 80% of business logic** — Look for:
   - Service/use-case classes (contain orchestration and rules)
   - Domain model classes (contain entities and state transitions)
   - Validation classes (contain business rules)
   - Database migrations (contain entity structure)
   - Test files (contain behavioural evidence)
4. **Prioritise those files** for deep reading. Deprioritise:
   - Generated code, DTOs with no logic, infrastructure boilerplate
   - Configuration-only files (once architecture is understood)
   - UI styling, asset files, build scripts

### Principle 2: Use Search Tools Over Bulk Reading

Prefer targeted discovery over reading entire files:

- **`grepSearch`** to find specific patterns across the codebase:
  - All validation functions: search for `validate`, `check`, `assert`, `guard`
  - All state transitions: search for `status`, `state`, `transition`, `lifecycle`
  - All external calls: search for `HttpClient`, `fetch`, `axios`, `RestTemplate`, `WebClient`
  - All error handling: search for `throw`, `catch`, `Exception`, `Error`, `fail`
  - All event publishing: search for `emit`, `publish`, `dispatch`, `send`, `produce`
  - All scheduled jobs: search for `@Scheduled`, `cron`, `schedule`, `job`
- **`readCode`** to get function signatures and class structure without reading full implementations
- **`readFile` with line ranges** when you need a specific function body identified by search

### Principle 3: Work Domain-by-Domain

For large codebases with multiple domains or modules:

1. **Identify domains** from folder structure and naming (e.g. `recommendation/`, `reservation/`, `approval/`)
2. **Analyse one domain at a time:**
   - Read relevant files for that domain
   - Extract evidence for that domain
   - Record findings in an internal domain evidence summary
3. **Do not hold multiple domains in context simultaneously** — analyse domain A, record findings, then move to domain B
4. **Compose the artefact** from domain-level evidence summaries after all relevant domains are inspected

### Principle 4: Build an Evidence Index

For large codebases, maintain an internal evidence index as you inspect:

```
Domain: recommendation
  Key files: recommendation-service.kt, recommendation-repository.kt, recommendation-events.kt
  Business rules found: 5 (BR-001 to BR-005)
  Entities found: 3 (UserWorkspace, UserTransaction, User)
  Integrations found: 2 (Airlines, Airport)
  Open questions: 2

Domain: Ticket Reservation
  Key files: reservation-service.kt, reservation-validator.kt
  Business rules found: 3 (BR-006 to BR-008)
  Entities found: 2 (Reservation, FundingSource)
  Integrations found: 1 (Bank)
  Open questions: 1
```

This index is not written to a file — it is the agent's internal working memory for composing the artefact. It allows the agent to reference evidence without re-reading source files.

### Principle 5: Delegate Deep Investigation to Sub-Agents

When a specific module requires deep analysis and the current context is already substantial:

- Use the `context-gatherer` sub-agent to investigate a specific module or file set
- Provide it with a focused question (e.g. "What business rules exist in the recommendation-service?")
- Incorporate its findings into your evidence index

### Principle 6: Incremental Artefact Production

For large codebases, produce artefacts incrementally within a single artefact:

1. Analyse domain A → write partial findings to the artefact file
2. Analyse domain B → append findings to the same artefact file
3. After all domains → review the complete artefact for consistency

This avoids holding all evidence in context at once — the file system serves as intermediate storage.

### When to Apply This Protocol

**Detection step (mandatory during artefact 01):**

Before beginning deep code inspection, run a size assessment:

1. Use `listDirectory` with depth on the repository root to count source folders and files.
2. If the repository clearly has more than ~100 source files or multiple service folders, activate this protocol.
3. Record in `.progress.json`:
   ```json
   "largeCodebase": true,
   "estimatedSourceFiles": 250,
   "domains": ["recommendation", "reservation", "approval"]
   ```
4. If `largeCodebase` is `true`, apply this protocol for ALL subsequent artefacts — not just artefact 01.

**Trigger criteria:**

- The repository has more than 100 source files (excluding tests, configs, generated files)
- The repository has more than 20,000 lines of source code
- The repository is a monorepo with multiple services
- Multiple repositories are being analysed together
- Context compaction has already occurred once during the current artefact

**If uncertain, default to applying the protocol.** The overhead of being systematic is lower than the cost of losing context mid-analysis.

**For smaller codebases** (fewer than 100 source files, single service): The agent may read files more freely without strict domain-by-domain partitioning, but should still prefer search tools over bulk reading.
