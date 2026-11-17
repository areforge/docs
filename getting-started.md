---
title: Quickstart
---

# Quickstart

This assumes an Areforge instance and the app are already running. The goal is the first review on a real merge request, typically under 15 minutes once you have access.

## 1. Sign in

Open the app and go to `/{locale}/login` (for example `/en/login` or `/fa/login`). Login is OTP to a mobile number (`09xxxxxxxxx`). The first successful OTP **creates the account**.

Add an email from **Account** if you will accept invitations. Invitations are sent by email and attach to the signed-in account whose email matches.

## 2. Add the repository

In the app: **Add repository**. Two steps: where the repository lives, then how it should be reviewed.

**Host:** GitLab or GitHub.

**Repository identifier**

| Host | What to enter |
|---|---|
| GitLab | The numeric project ID, or the full path such as `group/repo` |
| GitHub | `owner/repo` |

**Access token.** A git-host token that can **read the code and post comments** on this repository. Areforge stores it encrypted and never shows it again. See [GitLab](./setup/gitlab.md) or [GitHub](./setup/github.md) for scopes.

**GitLab host (optional).** For self-hosted GitLab, set the server URL (for example `https://gitlab.example.com`). Leave it empty to use gitlab.com.

**Name.** The label you will see in Areforge. It does not have to match the git path.

**Stack profile (optional).** Tunes review guidance to the stack. Registered keys today: `laravel`, `nodejs`, `nextjs`, `vue`, `dotnet`, `go`, `wordpress`, `django`, plus generic backend/frontend. You can continue without one. Details: [Stack profiles](./configuration/stack-profiles.md).

**Ownership.** Personal (only you) or a team you already belong to. The team cannot be changed after create. To share a repository, [create an organization and a team](./app/organizations.md) first.

Custom guidance, ignore patterns, review language, and the quiet/balanced/thorough profile are set later on the repository’s review-settings screen, or in [`.areforge.code-review.yaml`](./configuration.md).

## 3. Save the CI token

On success the app shows:

- A **CI token**, shown **exactly once**. There is no way to read it again. If you leave without saving it, rotate it later from [Credentials](./setup/credentials.md).
- A **CI snippet** generated for this repository. The `POST` URL is already the Areforge origin. `X-Repository-Id` is already the identifier you registered. The Bearer variable in the generated snippet is `AI_PLATFORM_TOKEN`.

Put the token in that repository’s CI/CD variables as `AI_PLATFORM_TOKEN` (masked, protected if your host supports it), and paste the snippet into CI. Full job: [Connect CI](./ci.md).

## 4. Open or push an MR

The job runs on a merge-request / pull-request pipeline and `POST`s `/api/v1/code-review`. The API answers `202` with `Review queued` (or `Already requested` if that head SHA is already in flight).

On the MR you should then see:

- Inline comments on changed lines
- A summary note: files reviewed, comment count, advisory **PR Risk** 0–100, skipped-file details, and a sequence diagram when several components actually interact
- In the app, the same run under the repository’s **Reviews** tab. Over time, sequence diagrams merge into the [repository system map](./system-map.md)

## If nothing arrives

See [Troubleshooting](./troubleshooting.md). The usual causes are a missing snippet, a mismatched `AI_PLATFORM_TOKEN`, a wrong `X-Repository-Id`, or a git-host access token that cannot post notes.
