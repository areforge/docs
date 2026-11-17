---
title: Cost and model routing
---

# Cost and model routing

Token cost is controlled in layers **before** a file reaches the model, then recorded after. SaaS competitors bill per seat; here the filters are in your configuration and every run reports what it spent.

## Filters before the model

| Layer | Default | Effect |
|---|---|---|
| Ignore patterns | locks, minified, vendor, node_modules, dist, build, migrations, svg, snapshots | Matching files never reach the model |
| Include patterns | empty (all files) | A non-empty allowlist **narrows** the set |
| Minimum changed lines | 8 | Smaller diffs are skipped |
| Max lines per file | 1500 | Oversized / generated files are skipped |
| Max files per run | 25 | Largest diffs first; overflow is skipped. Instance-wide — a repository cannot raise this |
| Retry | — | A file that already produced a result is not sent again |

All of these except the file cap are [review settings or YAML](../configuration.md).

## Per-file model tier

When model selection is enabled on the instance, each file is assigned a **tier**:

| Tier | When | Typical model |
|---|---|---|
| Strong | Path matches `sensitive_patterns` (auth, security, payment, billing, transaction) | opus-class |
| Cheap | Non-sensitive, and changed lines ≤ `trivial_max_changed_lines` (default 10) | haiku-class |
| Default | Everything else | sonnet-class |

Sensitive always wins over trivial. The concrete model IDs are instance settings. A repository cannot pick a model; it can only add sensitive globs and tune the trivial-line ceiling.

When selection is disabled, every file uses the default tier.

## What is recorded

For each reviewed file Areforge stores input tokens, output tokens, and the model that served. Repository [insights](../app/insights.md) roll that up:

- Tokens in / out
- Cost by model (from instance prices)
- Average tokens per MR
- Estimated engineering time saved and ROI, using the instance minutes-saved assumption and the organization (or personal-owner) **senior hourly rate**

If a model has no configured price, its tokens are left out of the total and the UI says so — the real cost is then higher than the figure shown.

## ROI framework

These numbers are filled in from your own project: real provider usage, MR count, and your team’s hourly rate. Areforge does not invent them.

```text
AI Cost = MR Count × Average Tokens (post-filter) × Token Price
Engineering Saving = Review Hours Saved × Senior Hourly Rate
ROI = Engineering Saving ÷ AI Cost
```

The cost field is `total_cost_usd` over the selected range (or all time), not a calendar-month invoice.
