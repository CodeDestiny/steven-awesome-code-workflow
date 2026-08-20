# Risk and Review

Load this reference after discovery for every non-trivial task. Treat it as the fallback source for decision gates, task routing, human authority, and reviewer topology. Apply the repository's scoped model whenever it exists, whether it is stricter, lighter, or differently structured; route any conflict with higher-level instructions to `Q0 / BLOCK`.

## Clarification Gates

Resolve facts from project evidence and reserve questions for decisions:

- `Q0`: blocks the Plan because the outcome, authority, or safe boundary is undefined.
- `Q1`: changes scope, contract, risk, or acceptance; close it or record an explicit human downgrade.
- `Q2`: does not block execution; record it with its follow-up.

For PRDs, prototypes, screenshots, demos, or incomplete requirements:

1. Use `grilling` when available; otherwise use the same manual pattern.
2. Ask one decision at a time, include a recommended answer, and wait.
3. Keep included inputs, excluded inputs, non-goals, and defaults explicit.
4. Obtain confirmation of shared understanding before the development Plan.

While a `Q0` or blocking `Q1` remains, complete the turn with current evidence, a provisional route, exactly one decision question with a recommended answer, and status `BLOCKED_BEFORE_PLAN`. Enter planning on a later turn after those decisions close.

Require traceable human confirmation for these semantics:

| Decision | Authority |
|---|---|
| Product behavior, state meaning, user result | Product or domain owner |
| Money, refunds, billing, settlement, entitlement | Domain owner plus accountable financial owner |
| Authorization model or security risk | Security owner |
| Schema, history, migration, backfill, recovery | Data owner plus affected domain owner |
| Destructive public architecture or contract | Accountable technical owner |
| Significant residual production risk | Owner accountable for the production surface |

Record `confirmed_by`, `authority`, `evidence`, and `confirmed_at`. Keep residual-risk acceptance independent from implementation ownership.

## Task Axes

Record independently:

| Axis | Values | Controls |
|---|---|---|
| `risk_tier` | `Small / Normal / High-risk` | Safety and validation floor |
| `change_mode` | `Standard / Hotfix` | Sequencing and urgency |
| `effort_size` | `S / A / B / C / D` | Decomposition and resources |
| `review_lane` | `Direct / Fast / Guarded / Audit` | Plan checkpoint, reviewers, evidence detail |

Risk tiers:

- `Small`: docs, comments, formatting, or local tests with no changed production behavior, public contract, oracle, coverage policy, CI, test infrastructure, security policy, or release gate.
- `Normal`: bounded, reversible behavior with clear acceptance and faithful automated evidence.
- `High-risk`: money, entitlement, permission; usable authentication secrets or a changed sensitive-data boundary such as new/expanded flows, sinks, carriers, audiences, retention, export/backup, anonymous or cross-tenant access, or AI reachability; destructive contract, schema/history, message/job semantics, transaction/concurrency/idempotency, cross-system write, batch users, recovery invariant, or critical observability.

Treat CI, test infrastructure, quality gates, security policy, and release workflow as at least `Normal`. Support any apparent High-risk downgrade with code evidence.

Do not route solely on the words “sensitive” or “raw”. A representation-only change to a non-credential field need not escalate when a scoped repository policy already authorizes it and the sink, audience, retention, export/backup, and AI reachability remain unchanged. Unknown or unapproved destinations do not inherit that permission; repository security and logging policy remains authoritative.

## Risk Card

| Dimension | Required evidence |
|---|---|
| Outcome | Required user/system result and forbidden behavior |
| Scope | Entries, domains, consumers, configuration, jobs, messages, users |
| Contract | API, DTO, enum, error, JSON, event, version compatibility |
| Data | Schema/history, stop, resume, reconcile, rollback or forward repair |
| Side effects | Charge, notify, publish, schedule, cross-system write, safe repetition |
| Consistency | Transaction, lock, retry, duplicate, order, partial success, compensation |
| Security | Authn/authz, trust boundary, usable secrets, data flow/carrier/audience/retention/export/AI reachability, signature, dynamic execution, privilege |
| Detection | Logs, metrics, alerts, smoke, reconciliation |
| Recovery | Operator, trigger, stop control, executable recovery |
| Uncertainty | Assumptions and evidence that would overturn the route |

The card is complete when every applicable row has evidence, is explicitly `N/A`, or has an explicit unresolved decision. The card identifies risks; it does not require mechanisms for inapplicable rows.

Before adding conditional writes, optimistic/distributed locks, fencing, digests or frozen artifacts, compensation/recovery state, or new failure branches, cite at least one accepted requirement, scoped rule/contract, reachable evidenced failure path and consequence, or external protocol. Then use the minimum mechanism and record the failure mode, supporting evidence, why a simpler implementation is insufficient, and the validation that proves it.

## Review Lanes

| Lane | Fit | Checkpoint and reviewers |
|---|---|---|
| `Direct` | Small, docs, comments, local test maintenance | Short Plan/self-check; 0 reviewers |
| `Fast` | Clear, bounded, reversible, no external write, faithful targeted proof | Mini Plan; 0 reviewers |
| `Guarded` | Normal public/read boundary, error, performance, or acceptance uncertainty | Close `Q0/Q1`; maintain a Working Evidence Receipt; 1 stable reviewer: Implementation for production code, Architecture for governance/boundaries |
| `Audit` | High-risk or actual change to protected invariants | Architecture + Test review Plan, then explicit user approval; final Architecture + Implementation + Test; add Security for a changed trust or sensitive-data boundary |

Keep deterministic display, passthrough, ordering, filtering, and local compatible fixes in `Fast` while evidence remains clear. Route every actual High-risk trigger to `Audit`.

## Reviewer Model

| Reviewer | Sole responsibility |
|---|---|
| Architecture | Boundaries, contracts, data evolution, cross-system consistency, rollout/recovery |
| Implementation | Correctness, branches, transaction/concurrency/idempotency, performance, robustness |
| Test | Independent oracle, counterexamples, test layer, failure injection, regression/release evidence |
| Security | Threat model, authn/authz, trust and sensitive-data boundaries, injection/replay, signing/secrets, privileged tools |

Load specialist capabilities into these reviewers: `DomainContract` → Architecture; `DataMigration` → Architecture + Test; `ReliabilityConcurrency` → Implementation + Test; `PerformanceCapacity` → Implementation; `OperabilityRelease` → Architecture + Test.

Keep reviewers independent: separate them from implementation, supply raw artifacts, require an independent risk view, keep the reviewer topology fixed, and let the finding owner or another reviewer verify closure. Reuse the same persistent reviewer for targeted re-review when available. Reviewer updates should contain only new findings, evidence gaps, unreviewed areas, or evidence that changes a conclusion; do not restate requirements, diffs, or unchanged logs. Resolve disagreements with reproducible evidence and authorized human decisions. Count silence or unavailable required review as `NOT_COMPLETED`, which yields `BLOCK`.

## Optional Review Helper

Use `code-review` only inside the Implementation review when at least one trigger is evidenced: cross-domain or public-contract scope; acceptance-to-implementation mapping remains unclear after the oracle is confirmed; a required reviewer reports low confidence; the same class of finding repeats; or the user explicitly requests a second review axis.

The helper returns candidate issues only. It is not a Reviewer, cannot assign or close finding severity, cannot create additional reviewers, and its absence or failure is not a gate unless it exposes a real evidence gap. Required Reviewers own triage, findings, and closure.

## Reroute

Rebuild the card and lane when the diff introduces irreversible data, an external write, privilege, an undefined oracle, scope drift, a public contract change, unavailable faithful validation, or a repeatedly unclosed blocking finding.

Keep the original risk tier during a Hotfix. Mark containment complete only after deferred verification, review ownership, and deadlines are explicit.
