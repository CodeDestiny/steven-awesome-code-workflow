# Evidence and Delivery

Load this reference for repository changes, formal review, or delivery conclusions. Treat it as the single source for required proof, findings, and delivery status.

## Contents

- [Conclusions](#conclusions)
- [Lane Gates](#lane-gates)
- [Change-Type Proof](#change-type-proof)
- [Evidence Records](#evidence-records)
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
| `Direct` | Relevant document/format/link/test/script check; final diff integrity; inline evidence in the delivery surface |
| `Fast` | Mini Plan; targeted automated behavior proof; compile/type proof for signature risk; final diff integrity; unchanged security boundary; inline evidence |
| `Guarded` | Fast gates; one reference-first Evidence Receipt or repository equivalent; one stable independent reviewer; closed `Q0/Q1` |
| `Audit` | Guarded gates; extended Receipt; broad regression; change-type proof; Architecture + Implementation + Test on the same final diff/evidence; conditional Security; rollout/recovery/observability |

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

## Evidence Records

Apply the repository's scoped evidence format first. When it has none:

- Keep `Direct/Fast` evidence inline in the PR, issue, task, or delivery summary.
- Maintain one reference-first Evidence Receipt or equivalent for `Guarded/Audit`; let it follow the ordinary branch, PR, and Git lifecycle rather than creating a separate state machine.

A default Receipt is deliberately small:

```yaml
task:
  risk:
  mode:
  effort:
  lane:
refs:
  requirement:
  diff:
gates:
  - name:
    acceptance: []
    result:
    evidence:
review:
  role:
  scope:
  result:
  findings: []
residual_risks: []
result:
```

Use this schema as the complete default, not as a checklist to expand. A detail being available is not a trigger to add it.

- Reference acceptance IDs from `requirement` in the applicable gates; keep acceptance prose in the requirement.
- Put change types, capabilities, the risk card, implementation scope, changed files, raw validation/review output, and reviewer input snapshots in their source artifacts or the delivery summary. Link evidence from the relevant gate or finding instead of adding parallel Receipt sections.
- Add commands and environment only when they are needed to reproduce a claim or explain an actual failure, retry, or flake.

Extend the default only when a scoped repository rule requires it or one of these conditions occurs: additional reviewers or specialized proof for `Audit`; a waiver for accepted S1 risk; release, stop, recovery, observability, or an observation window for a release surface; expiry for externally retained evidence.

Prefer references over copied requirements, diffs, changed-file lists, logs, or reviewer prose. Omit empty helper, waiver, release, and specialized-proof structures. Omit token use, elapsed time, review-round counts, and other process metrics unless a scoped repository rule makes them decision evidence.

Bind every evidence reference to the exact diff, tests, configuration, acceptance item, failure mode, and result it covers. Add commands and environment only when needed to reproduce the claim or explain an actual failure, retry, or flake. Share unchanged bound evidence across reviewers. After a fix, rerun affected gates and every gate whose binding changed.

For non-reproducible production observation, one-time data checks, asynchronous ordering, or external approval, retain sanitized key evidence or a controlled reference with an expiry. Keep sensitive, large, or high-frequency output outside Git.

The evidence record is complete when every required gate and acceptance item has a traceable result or an explicit `BLOCK`.

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
3. Required-gate result summaries and evidence references; commands and environments only when needed for reproduction or to explain an actual failure, retry, or flake.
4. External changes, rollout, stop, recovery, observability.
5. Reviewers, scope, findings, closure owners, waivers, unreviewed areas.
6. Residual risk and required follow-up.
7. Actual state: code-complete, committed, pushed, merged, released, observed, or closed.
8. Decisions, tradeoffs, problems, improvements, reusable knowledge.

Delivery is complete when the stated status is directly supported by evidence.
