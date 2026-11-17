---
title: Troubleshooting
---

# Troubleshooting

## Nothing arrives on the merge request

Walk this list in order.

1. The snippet is at the repository root (GitLab: `.gitlab-ci.yml`; GitHub: `.github/workflows/…`) and the pipeline ran on the MR / PR event.
2. `AI_PLATFORM_TOKEN` matches the value shown at create (or the last rotation). A recreate of the variable with the old value still works; a typo does not.
3. `X-Repository-Id` is the identifier you registered, not a different path or id. GitLab numeric id and `group/repo` are not interchangeable unless that is what you typed at register.
4. The git-host **access token** can still read the project and post notes. Check Credentials: status should be **Working**.
5. The Areforge URL in the snippet is reachable from the CI runner (self-hosted runners on a private network must be able to reach the instance).
6. The API answered `202`. A `401` is headers; a connection error never queued a review.

The app **Reviews** tab is the source of truth for whether a request was recorded. Empty there means the `POST` never authenticated.

## `Already requested`

That `head_sha` is already stored for this repository. Push a new commit, or you are looking at a retried pipeline on the same SHA — both are expected.

## `401 Missing repository id or token` / `Invalid credentials`

Both `Authorization: Bearer …` and `X-Repository-Id` are required. The header is `X-Repository-Id`, not `X-Project-Id`. After a CI-token rotation, the old bearer is invalid immediately.

## Comments missing on a file

The file was probably skipped. The MR summary has a collapsed **Skipped file details** list, and the app review page lists the same reasons: ignore pattern, include miss, below the line threshold, too large, over `max_files_per_run`.

A second push will not re-post a finding that already exists on that MR ([Memory](./reviews/memory.md) / fingerprint). Look for the original discussion.

## Inline comment became a general note

The git host rejected the line position (rebase, binary, or a line that is not in the diff). Areforge falls back to a note naming `file:line`. The finding is still recorded in the app.

## Access token “Not working”

Revoked, expired, or missing scope. Fix it at GitLab / GitHub, then **Replace token** or **Check again** on [Credentials](./setup/credentials.md). New reviews will not record until this is working.

## Lost the CI token

Do not recreate the repository. **Issue a new token** on Credentials, put it in `AI_PLATFORM_TOKEN`, and run the next MR pipeline.

## No sequence diagram / empty system map

Expected when the change is one file, below the volume threshold, or the diagram agent decided there was no real interaction. A newly connected repository stays empty until diagrams have merged. See [System map](./system-map.md).

## Dashboard disagrees with the live review

The settings screen shows repo → org → default. It does **not** include `.areforge.code-review.yaml`. The file is read from the **base** branch at run time. See [Review configuration](./configuration.md).

## Invitation will not accept

Sign in first. The signed-in account’s **verified email** must match the invitation. Add email from [Account](./app/account.md). Expired or revoked links need a new invite.

## GitHub comments landed on the wrong commit

The workflow used `github.sha` instead of `github.event.pull_request.head.sha`. Use the generated snippet. See [GitHub](./setup/github.md).
