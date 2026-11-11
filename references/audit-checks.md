# Risk-Based Technical Debt Audit Checks

Read for `deep` audits, high-risk workflows, or a specific category. This is a selection menu, **not** a demand to run every check on every task. Inspect the actual stack and current requirements before selecting tools.

## 1. Knowledge continuity and AI independence

- Can a new engineer start the app, trace a representative transaction, find the change point, and validate the result from repository files alone?
- Are generated components understandable, tested, and owned by the project rather than an agent's conversation history?
- Are key business invariants explicit, executable when possible, and easy to locate?
- Are meaningful architecture trade-offs recorded briefly, with revisit conditions?
- Do docs contradict code? Are comments explaining **why** instead of narrating what the code obviously does?
- Is an unusual pattern justified by current requirements rather than AI output conventions?

## 2. Architecture and application code

- Unnecessary layers, abstractions, delegation, generic configurations, overgrown functions and modules, circular dependencies.
- Duplicate business rules vs. harmless local duplication; unexpected side effects; hidden global state; accidental shared mutable state.
- Error propagation, cancellation, timeouts, concurrency, retries, idempotency, and lifecycle boundaries.
- API versioning, input/output contracts, pagination, status/error semantics, frontend-backend schema drift.
- Features implemented but unreachable; code used only by obsolete paths; compatibility shims without active consumers.
- Lack of an explicit reason for a separate service, database, queue, or framework.

## 3. Tests and change safety

- Are there meaningful unit, integration, and contract tests for expensive-to-break flows?
- Do tests check the real contract (including error cases), rather than simply mirror the current implementation?
- Are tests deterministic, isolated, reasonably fast, and usable from a clean environment?
- Is CI reproducible? Do lint/type/build/test commands agree with documented workflows?
- Are critical regressions protected before refactoring? Are failures distinguishable from environment limitations?

## 4. Security, privacy, and tenant isolation

- Authentication is not authorization. Verify resource ownership for each sensitive read/write/action.
- Cross-tenant, cross-doctor, cross-company, and secretary/delegate access where relevant.
- Object-level authorization, mass assignment, trusted input, query safety, rate-limiting where needed, and least privilege.
- Credential handling, secret exposure, logging redaction, dependency vulnerability management, and software supply chain.
- Sensitive-data retention, encryption requirements, auditability, and privacy boundaries.
- Do not automatically exploit live systems or reveal actual protected data. Use fixtures/authorized tests.

## 5. Data correctness and migrations

- Transaction boundaries, isolation, uniqueness, foreign keys, concurrent writes, and idempotency.
- Migration ordering, backward compatibility, safe retries, schema changes across running versions, and rollback/roll-forward plans.
- Backup integrity, *tested* restore paths, deletion/retention policies, and operational failure recovery.
- Distinguish database atomicity from external files, messages, remote APIs, and separate sessions.

## 6. Dependencies and delivery

- Runtime/toolchain versions, unsupported packages, unused dependencies, lockfiles, licensing and reproducibility.
- Dependency update path, stale build tasks, fragile generators, hard-coded environment assumptions.
- Release gates, deployment checks, health checks, rollback strategy, misconfiguration detection.
- Production readiness claims require concrete verification, not assumptions from a local build.

## 7. Performance and reliability

- Profile or measure before optimization; do not label unfamiliar code "slow" without evidence.
- N+1 queries, unnecessary network hops, costly polling, leaked handles, caches without invalidation, bounded memory/concurrency.
- Observability for relevant failures: useful logs, counters/traces where appropriate, no sensitive payload logging.
- Manual repeated steps (operational toil), noisy alerts, and recovery steps requiring undocumented knowledge.
- Favor proportionate design for actual scale; do not demand distributed infrastructure where a direct call suffices.

## 8. Frontend, mobile, and accessibility

- Duplicate sources of truth, derived-state bugs, effects that could be ordinary computation, unnecessary rerenders.
- Client/server validation mismatch, misleading loading and error states, fragile offline/retry behavior.
- Semantic controls, keyboard/focus paths, screen reader labels, contrast, form errors, localization.
- Platform/version compatibility, bundles and dependencies justified by user-visible value.

## Issue classification (required)

- `debt`: a current trade-off that increases the cost or risk of future change.
- `defect`: wrong current behavior.
- `vulnerability`: exploitable security weakness or exposure.
- `limitation`: known unsupported capability or environment boundary; may or may not be debt.
- `accepted trade-off`: conscious cost with rationale and revisit trigger.
- `improvement`: optional opportunity not shown to impose material costs.

For each: mark `confirmed` / `likely` / `unverified`; include evidence and a verification plan. Severity and confidence are distinct. Do not turn an unverified hypothesis into a P0 simply by using frightening language.
