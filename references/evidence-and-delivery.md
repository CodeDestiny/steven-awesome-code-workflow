# Evidence and Delivery

Load this reference for repository changes, formal review, or delivery conclusions. Treat it as the single source for required proof, findings, and delivery status.

## Contents

- [Conclusions](#conclusions)
- [Lane Gates](#lane-gates)
- [Change-Type Proof](#change-type-proof)
- [Evidence Manifest](#evidence-manifest)
- [Findings](#findings)
- [Audit Challenge](#audit-challenge)
- [Delivery](#delivery)

## Conclusions

| Conclusion | Criterion |
|---|---|
| `PASS` | Every required gate passed; blocking findings independently closed; no unaccepted critical risk |
| `CONDITIONAL_PASS` | Every required gate passed; remaining S1 has compensating evidence, authorized owner, scope, reason, expiry, tracking item, and recovery trigger |
| `BLOCK` | S0, missing/failed required gate, unconfirmed oracle, or evidence gap over a core failure mode |

Only completed required gates can lead to `PASS` or `CONDITIONAL_PASS`.

## Lane Gates

Apply repository-specific gates first:

| Lane | Required proof |
|---|---|
| `Direct` | Relevant document/format/link/test/script check; final diff integrity |
| `Fast` | Mini Plan; targeted automated behavior proof; compile/type proof for signature risk; final diff integrity; unchanged security boundary |
| `Guarded` | Fast gates; Evidence Manifest; one stable independent reviewer; closed `Q0/Q1` |
| `Audit` | Guarded gates; broad regression; change-type proof; Architecture + Implementation + Test on the same final diff/evidence; conditional Security; rollout/recovery/observability |

## Change-Type Proof

Use the most faithful reasonable layer:

| Type | Minimum proof |
|---|---|
| `behavior` | Main path, important boundary, smallest counterexample, forbidden side effect |
| `contract` | Old/new consumers, defaults, enums, errors, serialization, version compatibility |
| `query` | Filters, boolean grouping, order, pagination, query count, material plan/index behavior |
| `transaction` | Real transaction boundary; final state and side effects at each important failure |
| `data/migration` | Dry-run, batching, idempotency, stop/resume, counts, sample reconciliation, recovery |
| `async/job` | Duplicate, disorder, retry, partial failure, restart, cursor/offset, poison input, stop |
| `external-call` | Success, business failure, timeout, exception, partial success, retry/idempotency, degradation |
| `security` | Horizontal/vertical authorization, replay, injection, sensitive logs, deny-by-default, least privilege |
| `performance` | Query/call counts, bounded collections, plan/index evidence, capacity or repeatable benchmark |
| `release` | Order, feature/stop controls, smoke, logs/metrics/alerts, reconciliation, observation, tested recovery |

Mocks prove local behavior; production contracts require a faithful contract layer. Lightweight databases prove their own dialect and isolation; use production-faithful evidence for production claims. Mark an unavailable required layer `BLOCK`.

## Evidence Manifest

Maintain one manifest or equivalent task record containing:

- goal, non-goals, scope, acceptance items, baseline and final diff identity
- task axes, change types, capabilities, affected entries/consumers/configuration/data/side effects
- each command's environment, configuration, input snapshot, first result, retries/flakes, final result, raw evidence location
- acceptance item and failure mode mapped to bound evidence
- reviewer identity, responsibility, scope, findings, evidence gaps, unreviewed areas, overturn conditions
- external changes, rollout, stop, recovery, observability, observation window
- skipped gates, residual risk, waiver owner, deadline, tracking item

Bind evidence to the exact diff, tests, configuration, environment, acceptance item, and failure mode it covers. Share unchanged bound evidence across reviewers. After a fix, rerun affected gates and every gate whose binding changed.

The manifest is complete when every required gate and acceptance item has a bound result or an explicit `BLOCK`.

## Findings

Record:

```text
finding_id:
reviewer:
category: correctness | contract | data | security | reliability | performance | maintainability | evidence
severity: S0 | S1 | S2
confidence: high | medium | low
status: OPEN | FIXED_PENDING_VERIFY | VERIFIED | FALSE_POSITIVE | RISK_ACCEPTED | DEFERRED
reviewed_scope:
violated_invariant:
location:
evidence_or_counterexample:
impact:
minimal_fix:
acceptance_item:
owner:
verifier:
deadline:
```

- `S0`: credible money, data-integrity, security, destructive-contract, or core-correctness failure; always blocks.
- `S1`: credible user, reliability, performance, or recovery risk; fix or obtain time-bounded authorized acceptance after every gate passes.
- `S2`: low-risk maintainability or evidence improvement; track without blocking unless repository rules raise it.

Transitions:

```text
OPEN → FIXED_PENDING_VERIFY → VERIFIED
OPEN → FALSE_POSITIVE
OPEN → RISK_ACCEPTED   # S1 only
OPEN → DEFERRED        # S2 or documented non-blocking Hotfix follow-up
```

The implementer may mark `FIXED_PENDING_VERIFY`; the finding owner or another independent reviewer owns final closure.

## Audit Challenge

For `Audit`, require every reviewer to state critical invariants, the most dangerous unverified assumption, one failure sequence or minimal counterexample, the test that fails when a protection is removed, evidence that overturns `PASS`, and unreviewed areas.

## Delivery

Report:

1. Implemented outcome and affected surfaces.
2. Task axes, lane, change types, capabilities.
3. Exact commands, environments, first/final results, evidence locations.
4. External changes, rollout, stop, recovery, observability.
5. Reviewers, scope, findings, closure owners, waivers, unreviewed areas.
6. Residual risk and required follow-up.
7. Actual state: code-complete, committed, pushed, merged, released, observed, or closed.
8. Decisions, tradeoffs, problems, improvements, reusable knowledge.

Delivery is complete when the stated status is directly supported by evidence.
