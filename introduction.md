---
sidebar_position: 1
title: Introduction
---

# Areforge

Areforge is a **self-hosted AI code-review service** for GitLab and GitHub. On every push to a merge request or pull request it reviews the diff **file by file**, posts **inline comments on the changed line**, writes a **summary note** (coverage, comment count, token usage, an advisory PR Risk score 0–100), and records the run in the app.

The service runs on your infrastructure. Diffs and comments stay on your network. The only outbound call is to the model gateway you configure.

## What a review does

- One job per file, in parallel. A failure in one file does not stop the others.
- Structured comments: line, severity, category, body — posted on the changed line.
- Per-file model routing on the live path (strong / default / cheap).
- Token usage recorded per file and per model, on the MR summary and in the app dashboard.
- Memory: if the same finding was reported on an earlier MR, a “said this before” note is attached. The finding still posts.
- When several components actually interact, a Mermaid **sequence diagram** is posted on that MR. Those diagrams merge over time into a **repository system map** (a flowchart) in the app.

## How you use it

1. Sign in to the Areforge app (OTP; the first successful login creates the account).
2. Register the GitLab or GitHub repository and save the CI token — it is shown **once**.
3. Put the generated snippet in that repository’s CI and define the token as a CI variable.
4. Open or push to an MR. The job `POST`s `/api/v1/code-review` and the review is queued (`202`).

Details: [Getting started](./getting-started.md) and [CI](./ci.md).

## What is already in the app

Home, repository reviews, KPIs, cost, the system map, and objectives (OKR) attached to those metrics. Reporting is at repository, team, and organization scope.

## Still on the roadmap

An IDE extension, a CLI, built-in SAST, and auto-fix. Self-serve checkout is not wired; access and billing are arranged with you.

## Next

- [Getting started](./getting-started.md) — register a repository and see the first review
- [Connect CI](./ci.md) — the job, headers, and payload
- [Review configuration](./configuration.md) — `.areforge.code-review.yaml` and settings
- [Sequence diagrams and the system map](./system-map.md) — this-MR sequence, living repo flowchart
