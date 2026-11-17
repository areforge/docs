---
title: PR Risk
---

# PR Risk

PR Risk is a deterministic **0–100 advisory score** computed at the end of a review from that run’s own signals. There is **no extra model call** and **no merge gate**. The score is printed on the MR summary and stored with the review.

## Signals

Each signal adds its configured weight. The total is clamped to 100. Every non-zero contribution is listed as a human-readable factor, most impactful first.

| Signal | Default points | When it fires |
|---|---|---|
| Critical finding | 25 each | A `critical` comment was posted |
| Recurring finding | 20 each | [Memory](./memory.md) recognised the same issue on an earlier MR |
| Security-sensitive finding | 15 each | Category is `security`, `auth`, `payment`, `transaction`, `injection`, or `sql-injection` |
| Missing tests | 15 | Source files changed and no test-pattern file was in the run |
| Sensitive file touched | 12 each | The path matches `review.sensitive_patterns` (auth, security, payment, billing, transaction globs by default) |
| Large change | 10 | Total changed lines on reviewed files exceed `large_change_lines` (default 300) |
| Warning finding | 8 each | A `warning` comment was posted |

Weights are instance settings. A repository cannot change them.

## On the merge request

The summary note includes a line such as:

```text
🎯 PR Risk: 68/100 — 1 critical finding; 1 sensitive file touched; source changed with no test file touched
```

Up to three top factors are inlined. The rest are in the app.

## In the app

The review page shows the score, a low / moderate / high band, and the factor list. Repository, team, and organization [insights](../app/insights.md) roll up:

- Average PR Risk (only over scored requests)
- Count of **high-risk** MRs — default bar is **60**, instance-wide so the rollup stays comparable. A repository cannot lower that bar.

The score is a number out of 100, not a percentage.

## What it is not

- Not a merge check
- Not an AI judgement
- Not a ranking of authors
