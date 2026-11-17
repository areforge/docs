---
title: Insights
---

# Insights

Insights are KPIs computed on the server from raw review data. They are **not** a stored scorecard and **not** a ranking of people.

Scope:

| Where | What it covers |
|---|---|
| Repository → **Dashboard** | That repository |
| Team insights | Every repository the team owns, rolled up |
| Organization insights | The organization, then teams side by side |
| Account → insights / **My Growth** | Comments on **your** code |

Team comparison is ordered by how much was reviewed, not by score. A team with one reviewed MR at 100% coverage does not belong above a team with two hundred. A dash means nothing has been measured yet — not zero. Ratios are recomputed from raw totals, never averaged across repositories.

## Range

All time, or 7 / 30 / 90 days.

## Metrics

| Metric | Meaning |
|---|---|
| Reviews recorded | Completed review requests in range |
| Review coverage | Files reviewed / (reviewed + skipped) |
| Average time to first feedback | From the CI `POST` landing to the first comment on the code |
| Tokens spent | Input and output, and a breakdown by model |
| Suggestions accepted | Resolved discussions on **merged** MRs. This is the floor: resolve ≠ an explicit thumbs-up |
| Acceptance by category / severity | Same signal, split. Use it to tune guidance (for example “naming is almost never accepted here”) |
| Repeat findings | Share of findings [memory](../reviews/memory.md) had already raised |
| Comment categories | What the comments are about. Bars are against the largest category, not the total |
| Severity mix | Critical / warning / suggestion / nit |
| Merge request risk | Average PR Risk (scored requests only), and high-risk count (default bar 60) |
| Cost analysis | Total cost, estimated engineering time saved, ROI, average tokens per MR, price per 1,000 tokens, cost by model |

Coverage skipped files include ignore matches, tiny diffs, oversized files, and the per-run cap. A low coverage number on a lockfile-heavy MR is expected.

## Cost

See [Cost and model routing](../configuration/cost-and-models.md). If a model has no configured price, its tokens are omitted from the total and the UI says the real cost is higher.

## Individual insights

Your own page is private. An owner can open another member’s insights on that repository, for mentoring. Maintainers, reporters, and members cannot see other people’s individual data. Nothing here is wired to pay or ranking. See [Members](./members.md) and [Roles](./roles.md).
