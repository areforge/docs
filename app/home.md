---
title: Home
---

# Home

Home is the command center across every repository you can see. The path is `/{locale}/home`.

It is a **read model** from the server, not a client-side mash of many endpoints. Empty and waiting states are real product states.

## Empty and waiting

- **No repositories yet** — connect a GitLab or GitHub repository, or create an organization to share work with teammates.
- **Waiting for the first review** — the repository is connected, but no merge request has been reviewed yet. The next MR pipeline is enough.

## What it shows once reviews exist

| Block | Meaning |
|---|---|
| **Momentum** | Reviews in the period, high-risk count, how many items need attention |
| **Needs Attention** | Ordered by importance, not by time. Types: high-risk review, critical finding, recurring finding, acceptance dropped, coverage dropped, pending review |
| **Engineering Signals** | Risk, acceptance, coverage, reviews, critical findings — each against the previous period |
| **Recent AI Reviews** | What was reviewed most recently. Not a queue waiting on you |
| **Active Objectives** | Up to three current-quarter [objectives](./objectives.md) from repositories you can see |
| **My Growth** | Metrics actually attributed to **you**. Formative, not a ranking. Full page: Account → insights |
| **Top Repositories** | Shortcut into the repositories you use most |

Needs Attention is pay-only-on-drop for degradation: if overall acceptance or coverage did not fall, Areforge does not fan out per repository to hunt for a culprit.

## Range

The same 7 / 30 / 90 day (and all-time) window as [Insights](./insights.md).

## Next

- [Organizations and teams](./organizations.md)
- [Review history](./review-history.md)
- [Insights](./insights.md)
