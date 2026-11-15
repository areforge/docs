---
sidebar_position: 2
title: Getting started
---

# Getting started

This assumes an Areforge instance and the app are already running. The goal is the first review on a real merge request.

## 1. Sign in

Open the app and go to `/{locale}/login` (for example `/en/login` or `/fa/login`). Login is OTP. The first successful OTP **creates the account**.

## 2. Add the repository

In the app: **Add repository**. Two steps: where the repository lives, then how it should be reviewed.

**Host:** GitLab or GitHub.

**Repository identifier**

| Host | What to enter |
|---|---|
| GitLab | The numeric project ID, or the full path such as `group/repo` |
| GitHub | `owner/repo` |

**Access token.** A git-host token that can **read the code and post comments** on this repository. Areforge stores it encrypted and never shows it again.

**GitLab host (optional).** For self-hosted GitLab, set the server URL (for example `https://gitlab.example.com`). Leave it empty to use gitlab.com.

**Name.** The label you will see in Areforge. It does not have to match the git path.

**Stack profile (optional).** Tunes review guidance to the stack. Registered keys today: `laravel`, `nodejs`, `nextjs`, `vue`, `dotnet`, `go`, `wordpress`, `django`, plus generic backend/frontend. You can continue without one.

**Ownership.** Personal (only you) or a team you already belong to. The team cannot be changed after create.

Custom guidance, ignore patterns, review language, and the quiet/balanced/thorough profile are set later on the repository’s review-settings screen, or in [`.areforge.code-review.yaml`](./configuration.md).

## 3. Save the CI token

On success the app shows:

- A **CI token**, shown **exactly once**. There is no way to read it again. If you leave without saving it, you recreate the repository (or rotate credentials in settings, which issues a new token).
- A **CI snippet** generated for this repository. The `POST` URL is already the Areforge origin. `X-Repository-Id` is already the identifier you registered. The Bearer variable in the generated snippet is `AI_PLATFORM_TOKEN`.

Put the token in that repository’s CI/CD variables as `AI_PLATFORM_TOKEN` (masked, protected if your host supports it), and paste the snippet into CI. Full job: [Connect CI](./ci.md).

## 4. Open or push an MR

The job runs on a merge-request / pull-request pipeline and `POST`s `/api/v1/code-review`. The API answers `202` with `Review queued` (or `Already requested` if that head SHA is already in flight).

On the MR you should then see:

- Inline comments on changed lines
- A summary note: files reviewed, comment count, token usage, advisory **PR Risk** 0–100
- A sequence diagram **only** when several components actually interact (and sequence diagrams are enabled)

In the app, the same run appears under the repository’s reviews. Over time, sequence diagrams merge into the [repository system map](./system-map.md).

## If nothing arrives

- The snippet is at the repository root (GitLab: `.gitlab-ci.yml`; GitHub: `.github/workflows/…`).
- `AI_PLATFORM_TOKEN` matches the snippet and the value shown at create time.
- `X-Repository-Id` is the identifier you registered, not a different path or id.
- The git-host access token can still read the project and post notes.
- The Areforge URL in the snippet is reachable from the CI runner.
