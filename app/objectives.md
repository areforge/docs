---
title: Objectives
---

# Objectives

Objectives are quarterly OKRs on a **repository**. The server computes progress from the same metrics as [Insights](./insights.md). Path: `/{locale}/repositories/{identifier}/objectives`.

Owner and maintainer can create and delete them. Reporter can view. See [Roles](./roles.md).

## Create

A title, a Gregorian quarter, and at least one **key result**. Each key result is one metric, a baseline (value today), and a target.

Available metrics:

| Metric | Unit |
|---|---|
| Review coverage | percent |
| Suggestion acceptance rate | percent |
| Repeat-finding rate | percent |
| Time to first feedback | seconds |
| Average merge request risk | out of 100 |
| Total cost | USD |

Progress is `(current − baseline) / (target − baseline)`, clamped 0–100. The formula detects direction, so a **lower-is-better** goal (bring average PR Risk from 70 to 30) works the same as a higher-is-better one.

If the metric has no data yet, current value and progress stay empty.

Home shows up to three **active** (current-quarter) objectives from repositories you can see.

## What this is not

- Not a team-level or organization-level OKR. KPI building blocks are repository-scoped; attach the objective where the data lives.
- Not a people ranking. There is no per-developer objective.
- Not stored progress. The number is always recomputed from live KPIs.
