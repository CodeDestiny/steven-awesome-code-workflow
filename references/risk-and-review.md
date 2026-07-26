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
- `High-risk`: money, entitlement, permission, sensitive data, destructive contract, schema/history, message/job semantics, transaction/concurrency/idempotency, cross-system write, batch users, recovery invariant, or critical observability.

Treat CI, test infrastructure, quality gates, security policy, and release workflow as at least `Normal`. Support any apparent High-risk downgrade with code evidence.

## Risk Card

| Dimension | Required evidence |
|---|---|
| Outcome | Required user/system result and forbidden behavior |
| Scope | Entries, domains, consumers, configuration, jobs, messages, users |
| Contract | API, DTO, enum, error, JSON, event, version compatibility |
| Data | Schema/history, stop, resume, reconcile, rollback or forward repair |
| Side effects | Charge, notify, publish, schedule, cross-system write, safe repetition |
| Consistency | Transaction, lock, retry, duplicate, order, partial success, compensation |
| Security | Authn/authz, trust boundary, sensitive data, signature, dynamic execution, privilege |
| Detection | Logs, metrics, alerts, smoke, reconciliation |
| Recovery | Operator, trigger, stop control, executable recovery |
| Uncertainty | Assumptions and evidence that would overturn the route |

The card is complete when every applicable row has evidence or an explicit unresolved decision.

## Review Lanes

| Lane | Fit | Checkpoint and reviewers |
|---|---|---|
| `Direct` | Small, docs, comments, local test maintenance | Short Plan/self-check; 0 reviewers |
| `Fast` | Clear, bounded, reversible, no external write, faithful targeted proof | Mini Plan; 0 reviewers |
| `Guarded` | Normal public/read boundary, error, performance, or acceptance uncertainty | Close `Q0/Q1`; 1 stable reviewer: Implementation for production code, Architecture for governance/boundaries |
| `Audit` | High-risk or actual change to protected invariants | Architecture + Test review Plan, then explicit user approval; final Architecture + Implementation + Test; add Security for a changed trust boundary |

Keep deterministic display, passthrough, ordering, filtering, and local compatible fixes in `Fast` while evidence remains clear. Route every actual High-risk trigger to `Audit`.

## Reviewer Model

| Reviewer | Sole responsibility |
|---|---|
| Architecture | Boundaries, contracts, data evolution, cross-system consistency, rollout/recovery |
| Implementation | Correctness, branches, transaction/concurrency/idempotency, performance, robustness |
| Test | Independent oracle, counterexamples, test layer, failure injection, regression/release evidence |
| Security | Threat model, authn/authz, sensitive data, injection/replay, signing/secrets, privileged tools |

Load specialist capabilities into these reviewers: `DomainContract` → Architecture; `DataMigration` → Architecture + Test; `ReliabilityConcurrency` → Implementation + Test; `PerformanceCapacity` → Implementation; `OperabilityRelease` → Architecture + Test.

Keep reviewers independent: separate them from implementation, supply raw artifacts, require an independent risk view, keep the reviewer topology fixed, and let the finding owner or another reviewer verify closure. Resolve disagreements with reproducible evidence and authorized human decisions. Count silence or unavailable required review as `NOT_COMPLETED`, which yields `BLOCK`.

## Reroute

Rebuild the card and lane when the diff introduces irreversible data, an external write, privilege, an undefined oracle, scope drift, a public contract change, unavailable faithful validation, or a repeatedly unclosed blocking finding.

Keep the original risk tier during a Hotfix. Mark containment complete only after deferred verification, review ownership, and deadlines are explicit.
