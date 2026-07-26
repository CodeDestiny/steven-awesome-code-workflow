# Risk and Review Model

Use this model only when the target repository does not define a stricter or more specific route.

## Task Axes

Record the axes independently:

| Axis | Values | Purpose |
|---|---|---|
| `risk_tier` | `Small / Normal / High-risk` | Set the minimum safety and validation floor |
| `change_mode` | `Standard / Hotfix` | Set sequencing and urgency without lowering risk |
| `effort_size` | `S / A / B / C / D` | Guide decomposition and resource use when useful |
| `review_lane` | `Direct / Fast / Guarded / Audit` | Set Plan checkpoints, reviewer count, and evidence detail |

Classify risk from actual behavior and failure impact:

- `Small`: change only docs, comments, formatting, or local tests without changing production behavior, public contracts, the acceptance oracle, coverage policy, CI, test infrastructure, security policy, or release gates.
- `Normal`: make a reversible, bounded behavior change with clear acceptance and faithful automated evidence.
- `High-risk`: change money, refunds, entitlements, permissions, sensitive data, destructive public contracts, database schema or historical data, message or job semantics, transactions, concurrency, idempotency, cross-system writes, batch user operations, rollback invariants, or critical observability.

Treat CI, test infrastructure, quality gates, security policy, and release workflow changes as at least `Normal`. Record evidence for any downgrade from an apparent High-risk trigger.

## Risk Card

Answer each applicable question after clarification, after the Plan, and after the final diff:

| Dimension | Question |
|---|---|
| Outcome | What user or system result must hold, and what behavior is forbidden? |
| Scope | Which entries, domains, consumers, configurations, jobs, messages, and users are affected? |
| Contract | Does the change alter API, DTO, enum, error, JSON, event, or version-compatibility semantics? |
| Data | Does it alter schema or history, and how can execution stop, resume, reconcile, roll back, or move forward? |
| Side effects | Does it charge, notify, publish, schedule, or write to another system, and can it repeat safely? |
| Consistency | What are the transaction, lock, retry, duplicate, ordering, partial-success, and compensation rules? |
| Security | Does it change authentication, authorization, trust boundaries, sensitive data, signatures, dynamic execution, or privileged tools? |
| Detection | Which logs, metrics, alerts, smoke checks, or reconciliation expose failure? |
| Recovery | Who operates the stop switch and recovery steps, and under what trigger? |
| Uncertainty | Which facts remain assumptions, and what evidence would overturn the current route? |

## Review Lanes

Choose the lane independently from risk and effort:

| Lane | Default fit | Plan checkpoint | Independent review |
|---|---|---|---|
| `Direct` | `Small`, documentation, comments, or local test maintenance | Short Plan or self-check | None |
| `Fast` | Clear, bounded, reversible behavior with no external write and faithful targeted evidence | Mini Plan; continue when implementation is already authorized | None |
| `Guarded` | Normal change with a public display contract, cross-boundary read, error semantics, performance concern, or meaningful acceptance uncertainty | Pause only for unresolved `Q0/Q1` or material decisions | One persistent relevant reviewer at the stable final checkpoint |
| `Audit` | High-risk or actual change to money, permissions, schema/history, async semantics, concurrency, cross-system writes, batch users, recovery, or critical observability | Architecture and Test review of risk card and Plan, then explicit user confirmation | Architecture, Implementation, and Test reviewers on the same final diff; add Security only for a real trust-boundary change |

Use the default reviewer budget `Direct=0`, `Fast=0`, `Guarded=1`, and `Audit=3`, with one conditional Security reviewer. Load specialist capabilities into an existing reviewer instead of inventing extra personas.

Do not upgrade a clearly bounded deterministic sort, field passthrough, local compatible fix, or single-entry read-only filter out of `Fast` without code evidence of real uncertainty. Do not downgrade a real `Audit` trigger because implementation appears easy.

## Reviewer Responsibilities

Assign one primary responsibility to each reviewer:

- Architecture: module boundaries, public contracts, data evolution, cross-system consistency, rollout, and recovery.
- Implementation: correctness, complex branches, transactions, concurrency, idempotency, query and call performance, robustness, and maintainability.
- Test: independent acceptance oracle, counterexamples, test level, failure injection, regression, and release evidence.
- Security: threat model, authentication and authorization, sensitive data, injection and replay, signing and secrets, untrusted inputs, dynamic execution, and privileged tool use.

Load these capabilities when triggered:

- `DomainContract`: business meaning, public contracts, enum or error semantics, and compatibility.
- `DataMigration`: schema, stored JSON, backfill, dual-read or dual-write, resume, reconciliation, and recovery.
- `ReliabilityConcurrency`: transactions, locks, retry, duplicate or unordered async work, idempotency, and compensation.
- `PerformanceCapacity`: query or external-call counts, pagination, batching, indexes, limits, and capacity.
- `OperabilityRelease`: configuration, async deployment, rollout order, smoke, alerts, stop controls, and recovery.

Do not let reviewers delegate additional reviewers or decide business semantics. Decide conflicts using reproducible evidence and authorized human decisions, not a majority vote.

## Reviewer Independence

Require all of the following:

1. Keep the reviewer separate from the implementation.
2. Give the reviewer the raw request, applicable project rules, current code and diff, acceptance items, and original validation evidence.
3. Require the reviewer to form an independent risk view before reading the implementer's self-assessment.
4. Let the implementer mark a finding fixed, but let the finding owner or another independent reviewer verify closure.
5. Reuse the same reviewer for affected incremental re-review unless scope, contracts, data semantics, external effects, risk, or the acceptance oracle changes.

Mark required independent review `NOT_COMPLETED` and the task `BLOCK` when the execution environment cannot provide it. Do not treat timeout or silence as approval.

## Human Decision Authority

Require a traceable human confirmation for:

| Decision | Required authority |
|---|---|
| Product behavior, state meaning, and user result | Product or domain owner for the capability |
| Money, refunds, billing, settlement, or entitlement value | Product or domain owner plus the accountable financial owner |
| Authorization model or security risk acceptance | Security owner |
| Schema, history, migration, backfill, and recovery | Data owner plus the affected domain owner |
| Destructive public architecture or contract boundary | Accountable technical owner |
| Significant residual production risk | Owner accountable for the affected production surface |

Record `confirmed_by`, `authority`, `evidence`, and `confirmed_at`. Do not let the implementer accept their own residual production risk.

## Dynamic Escalation

Stop and reroute on any of these changes:

- new irreversible data behavior, external write, or privileged access
- conflicting or insufficient acceptance oracle
- diff outside the confirmed domain or entry points
- changed public contract or compatibility behavior
- unavailable faithful required validation
- repeated failure to close the same blocking finding

Do not use a hotfix to lower risk. Mark only containment complete until deferred verification, review, and follow-up ownership are explicit.
