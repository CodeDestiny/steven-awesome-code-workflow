# Evidence and Delivery Model

Use evidence to falsify important failure modes, not to accumulate test counts.

## Delivery Conclusions

- `PASS`: complete every required gate, verify all blocking findings, and leave no unaccepted critical risk.
- `CONDITIONAL_PASS`: complete every required gate, then retain only an explicitly accepted significant residual risk with compensating evidence, an authorized human owner, scope, reason, expiry, tracking item, and recovery trigger.
- `BLOCK`: report any critical finding, missing or failed required gate, unconfirmed acceptance oracle, or evidence gap that leaves a core failure mode untested.

Do not use `CONDITIONAL_PASS` to waive an unexecuted or failed required gate.

## Lane Gates

Apply repository-specific gates first, then use these defaults:

| Lane | Required evidence |
|---|---|
| `Direct` | Relevant document, format, link, test, or script check; final diff integrity check |
| `Fast` | Mini Plan; targeted automated behavior evidence; compile or type evidence when signatures can change; final diff integrity check; no new security boundary |
| `Guarded` | Every Fast gate; Evidence Manifest; one relevant independent reviewer at a stable checkpoint; no unresolved `Q0/Q1` |
| `Audit` | Every Guarded gate; broad regression; change-type evidence; final conclusions from Architecture, Implementation, and Test reviewers on the same final diff and evidence; conditional Security review; rollout, recovery, and observability evidence |

Treat any repository-declared required gate as required even when this default model would be lighter.

## Change-Type Evidence

Select the most faithful reasonable test layer for each affected type:

| Change type | Minimum evidence |
|---|---|
| `behavior` | Map accepted outcomes to assertions covering the main path, important boundary, smallest counterexample, and forbidden side effect |
| `contract` | Prove old and new consumer behavior, defaults, enums, errors, serialization, and version compatibility |
| `query` | Prove filters, boolean grouping, ordering, pagination, query count, and plan or index behavior when material |
| `transaction` | Exercise the real transaction boundary and verify final state and side effects at each important failure point |
| `data/migration` | Dry-run; batch and idempotency behavior; stop and resume; before/after counts; sampled reconciliation; rollback or forward repair |
| `async/job` | Duplicate, disorder, retry, partial failure, restart, cursor or offset movement, poison input, and stop behavior |
| `external-call` | Success, business failure, timeout, exception, partial success, retry, idempotency, and degradation semantics |
| `security` | Horizontal and vertical authorization, replay, injection, sensitive logs, deny-by-default, and least privilege |
| `performance` | Query and call counts, collection bounds, index or plan evidence, capacity limits, or repeatable benchmarks |
| `release` | Deployment order, feature or stop controls, smoke, logs, metrics, alerts, reconciliation, observation window, and tested recovery |

Do not let mocks prove a third-party contract or a lightweight database prove production dialect, isolation, locks, indexes, message order, or external-system behavior. Use a more faithful layer or mark the limitation `BLOCK`.

## Evidence Manifest

Maintain one manifest or equivalent task record. Do not create parallel review packets or duplicate ledgers.

Include:

- goal, non-goals, scope, and acceptance items
- baseline and final diff identity
- `risk_tier`, `change_mode`, `effort_size`, `review_lane`, change types, and capabilities
- affected entry points, consumers, configuration, data, and side effects
- command, environment, configuration, input snapshot, first result, retries, flaky behavior, final result, and raw evidence location
- mapping from each acceptance item and failure mode to evidence
- reviewer instance, responsibility, reviewed scope, new findings, evidence gaps, unreviewed areas, and conditions that would overturn the conclusion
- external changes, rollout, stop, recovery, observability, and observation window
- skipped or unavailable gates, residual risk, waiver owner, deadline, and tracking item

Bind evidence to the files or diff, tests, configuration, environment, acceptance item, and failure mode it actually covers. Reuse unchanged evidence across reviewers. Rerun only affected gates after a local fix unless the final diff invalidates the earlier evidence.

## Finding Model

Keep clarification severity separate from code finding severity.

Record findings with:

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

Use severity consistently:

- `S0`: confirmed or highly credible risk to money, data integrity, security, destructive contracts, or core correctness; always block and never accept.
- `S1`: credible user-facing, reliability, performance, or recovery risk; fix it or obtain a time-bounded authorized human acceptance after all required gates pass.
- `S2`: low-risk maintainability or evidence improvement; do not block unless repository rules say otherwise.

Use these transitions:

```text
OPEN -> FIXED_PENDING_VERIFY -> VERIFIED
OPEN -> FALSE_POSITIVE
OPEN -> RISK_ACCEPTED
OPEN -> DEFERRED
```

Allow `RISK_ACCEPTED` only for `S1` and `DEFERRED` only for `S2` or a documented non-blocking hotfix follow-up. Let the finding owner or another independent reviewer verify or reject closure.

## Adversarial Review

For `Audit`, require each reviewer to:

1. State the critical invariants and most dangerous unverified assumption.
2. Construct at least one failure sequence or minimal counterexample outside the happy path.
3. Identify which test must fail if a protection is removed, a condition is reversed, or an exception is swallowed.
4. State what evidence would overturn a `PASS`.
5. List unreviewed areas.

## Final Delivery

Report:

1. The implemented outcome and affected surfaces.
2. Task axes, lane, change types, and capabilities.
3. Evidence Manifest summary with exact commands, environments, first and final results, and evidence locations.
4. External changes, deployment, stop, recovery, and observability.
5. Reviewers, reviewed scope, findings, closure owners, waivers, and unreviewed areas.
6. Residual risk and any required follow-up.
7. Current status: code-complete, merged, released, observed, or closed.
8. A concise summary of decisions, tradeoffs, problems, improvements, and reusable knowledge.

Do not claim release, observation, or closure from code-complete evidence alone.
