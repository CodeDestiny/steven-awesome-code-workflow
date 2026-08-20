# Evidence and Delivery

Load this reference for repository changes, formal review, or delivery conclusions. Treat it as the single source for required proof, findings, and delivery status.

## Contents

- [Conclusions](#conclusions)
- [Lane Gates](#lane-gates)
- [Change-Type Proof](#change-type-proof)
- [Evidence Lifecycle](#evidence-lifecycle)
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
| `Guarded` | Fast gates; one reference-first Working Evidence Receipt or repository equivalent; one stable independent reviewer; closed `Q0/Q1` |
| `Audit` | Guarded gates; extended Working Receipt; broad regression; change-type proof; Architecture + Implementation + Test on the same final diff/evidence; conditional Security; rollout/recovery/observability |

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
| `security` | Horizontal/vertical authorization, replay, injection, usable-secret exposure, data carrier/audience boundaries, deny-by-default, least privilege |
| `performance` | Query/call counts, bounded collections, plan/index evidence, capacity or repeatable benchmark |
| `release` | Order, feature/stop controls, smoke, logs/metrics/alerts, reconciliation, observation, tested recovery |

Mocks prove local behavior; production contracts require a faithful contract layer. Lightweight databases prove their own dialect and isolation; use production-faithful evidence for production claims. Mark an unavailable required layer `BLOCK`.

## Evidence Lifecycle

Apply the repository's scoped evidence format first. When it has none, use two layers with different lifetimes.

### Working Evidence Receipt

Keep `Direct/Fast` evidence inline in the PR, issue, task, or delivery summary. For `Guarded/Audit`, maintain one reference-first Working Evidence Receipt in this order of preference:

1. An existing Draft PR, issue, task, or equivalent collaboration surface.
2. An ignored local `.codex/evidence/<task-id>.yaml` file in a shared workspace.
3. If neither exists, the minimal receipt fields embedded in the commit, PR, task, or delivery surface.

Never commit the ignored working draft by default. Use a compact fallback shape:

```yaml
version: 1
kind: working_evidence_receipt
task:
  id:
  risk:
  mode:
  effort:
  lane:
refs:
  requirement:
  baseline:
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
result: IN_PROGRESS
```

Use this schema as the complete default, not as a checklist to expand. Reference acceptance IDs instead of copying prose; keep the risk card, implementation scope, changed files, raw outputs, and reviewer input snapshots in their source artifacts. Add commands and environment only when needed to reproduce a claim or explain an actual failure, retry, or flake.

At task close, delete the Working Receipt, merge its minimal proof into the durable PR/issue/commit/delivery surface, or promote only the irreducible parts to a Retained Evidence Record.

### Retained Evidence Record

Retention is exceptional. Create a repository-tracked record only when at least one condition holds:

- the evidence is not reproducible, such as a production observation, one-time data check, asynchronous ordering result, or external approval;
- an open finding, waiver, blocker, or residual risk needs an auditable close condition; or
- no reliable PR, issue, CI, commit, or release artifact preserves the proof and a real audit need exists.

Lane, risk tier, reviewer count, or an available detail alone does not justify retention. Store retained records only in a repository-defined path, or a documented fallback such as `docs/evidence/` when the repository permits it. Include the retention reason, steward, review/expiry date, final commit and PR/issue references, close condition, and separate accountable risk/release owners when applicable. A record steward is not automatically the human risk or release owner.

```yaml
version: 1
kind: retained_evidence_record
task: {id: "", title: ""}
refs: {requirement: "", final_commit: "", pr_or_issue: ""}
retention: {reason: "", steward: "", review_at: "", expires_at: ""}
accountability: {risk_owner: "", release_owner: ""}
evidence: {summary: "", controlled_ref: ""}
status:
close_condition:
residual_risks: []
result:
```

Prefer references over copied requirements, diffs, logs, or reviewer prose. Keep sensitive, large, or high-frequency output outside Git. Omit empty helper, waiver, release, and specialized-proof structures, and omit process metrics unless repository rules make them decision evidence.

Bind every reference to the exact diff, tests, configuration, acceptance item, failure mode, and result it covers. Share unchanged bound evidence across reviewers. After a fix, rerun affected gates and every gate whose binding changed. The evidence lifecycle is complete when every required gate and acceptance item has a traceable result or an explicit `BLOCK`, and the Working Receipt has been closed.

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
