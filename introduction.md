---
slug: /
title: Introduction
---

# Areforge

Areforge is a **self-hosted AI code-review service** for GitLab and GitHub. On every push to a merge request or pull request it reviews the diff **file by file**, posts **inline comments on the changed line**, writes a **summary note** (coverage, comment count, an advisory PR Risk score 0–100), and records the run in the app.

The service runs on your infrastructure. Diffs and comments stay on your network. The only outbound call is to the model gateway you configure.

## How to use this documentation

The docs are grouped the way you actually work with Areforge:

- **Get started** — what the product does, a first review in a few minutes, and the path from CI `POST` to a comment on the line
- **Connect** — GitLab and GitHub tokens, the generated CI snippet, and how to rotate credentials
- **Reviews** — findings, PR Risk, memory of earlier MRs, sequence diagrams, and the repository system map
- **Configure** — dashboard settings, `.areforge.code-review.yaml`, stack profiles, and token-cost controls
- **The app** — Home, organizations and teams, members, review history, insights, objectives, roles, and your account
- **Reference** — data governance, troubleshooting, FAQ

If you are new, start with [Quickstart](./getting-started.md). If the pipeline is already posting, jump to [How a review works](./how-a-review-works.md).

## What a review does today

- One job per file, in parallel. A failure in one file does not stop the others.
- Structured comments: line, severity, category, body — posted on the changed line.
- An advisory **PR Risk** score 0–100 on the MR summary, with the reasons that produced it. It is not a merge gate.
- Memory: if the same finding was reported on an earlier MR, a “said this before” note is attached. The finding still posts.
- When several components actually interact, a Mermaid **sequence diagram** is posted on that MR. Those diagrams merge over time into a **repository system map** in the app.
- Token usage is recorded per file and per model, and shown in the app dashboard.

## What is already in the app

Home, repository reviews (with the stored diff and comments), KPIs and cost, the system map, and objectives (OKR) attached to those metrics. Reporting is at repository, team, and organization scope. Organizations, teams, invitations, and roles are live.

## Still on the roadmap

An IDE extension, a CLI, built-in SAST, and auto-fix. Self-serve checkout is not wired; access and billing are arranged with you.

## Next

- [Quickstart](./getting-started.md) — register a repository and see the first review
- [How a review works](./how-a-review-works.md) — from `POST` to comments
- [Connect CI](./ci.md) — the job, headers, and payload
- [Review configuration](./configuration.md) — `.areforge.code-review.yaml` and settings
