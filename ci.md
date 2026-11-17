---
title: Connect CI
---

# Connect CI

Reviews start when CI `POST`s the code-review endpoint for a merge request or pull request. The snippet the app generates at registration is the job you should paste. This page is the contract behind that snippet.

Host-specific paste targets: [GitLab](./setup/gitlab.md) (`.gitlab-ci.yml`) and [GitHub](./setup/github.md) (`.github/workflows/`).

## Endpoint

```http
POST /api/v1/code-review
```

Full URL is `{Areforge origin}/api/v1/code-review`. The generated snippet already contains this URL.

Response: **`202`**. Body message is `Review queued`, or `Already requested` when that head SHA is already recorded for the repository.

## Authentication

Both headers are required.

| Header | Value |
|---|---|
| `Authorization` | `Bearer {CI token}` |
| `X-Repository-Id` | The identifier you registered (GitLab numeric id or `group/repo`, GitHub `owner/repo`) |

Missing either header → `401` `Missing repository id or token`. Wrong pair → `401` `Invalid credentials`.

The CI token is hashed at rest and shown only at create (or rotation). `X-Repository-Id` is **not** `X-Project-Id`.

The generated snippet sets `X-Repository-Id` to the registered identifier and reads the Bearer token from `$AI_PLATFORM_TOKEN`. Name the CI variable the same thing, unless you edit the snippet.

## JSON body

All four fields are required.

| Field | Meaning |
|---|---|
| `merge_request_iid` | GitLab MR IID, or GitHub pull-request number (integer ≥ 1) |
| `source_branch` | Head branch |
| `target_branch` | Base branch |
| `head_sha` | 40-character commit SHA of the head. On GitHub use `github.event.pull_request.head.sha` — `github.sha` on a `pull_request` event is the throwaway merge commit. |

## GitLab

Paste the generated job into `.gitlab-ci.yml`. Shape:

```yaml
ai-code-review:
  stage: review
  image: curlimages/curl:latest
  rules:
    - if: $CI_PIPELINE_SOURCE == "merge_request_event"
  script:
    - >
      curl -sS -X POST "https://areforge.example/api/v1/code-review"
      -H "Authorization: Bearer $AI_PLATFORM_TOKEN"
      -H "X-Repository-Id: group/repo"
      -H "Content-Type: application/json"
      -d '{"merge_request_iid": '"$CI_MERGE_REQUEST_IID"',
           "source_branch": "'"$CI_MERGE_REQUEST_SOURCE_BRANCH_NAME"'",
           "target_branch": "'"$CI_MERGE_REQUEST_TARGET_BRANCH_NAME"'",
           "head_sha": "'"$CI_COMMIT_SHA"'"}'
  allow_failure: true
```

`allow_failure: true` so a review outage does not fail the pipeline. Define `AI_PLATFORM_TOKEN` as a masked CI/CD variable.

Use the snippet from the app, not this example: the URL and `X-Repository-Id` must be the ones issued for **this** repository.

## GitHub

Paste the generated workflow under `.github/workflows/`. Shape:

```yaml
name: AI Code Review
on:
  pull_request:
jobs:
  ai-code-review:
    runs-on: ubuntu-latest
    steps:
      - name: Request AI review
        env:
          AI_PLATFORM_TOKEN: ${{ secrets.AI_PLATFORM_TOKEN }}
        run: >
          curl -sS -X POST "https://areforge.example/api/v1/code-review"
          -H "Authorization: Bearer $AI_PLATFORM_TOKEN"
          -H "X-Repository-Id: owner/repo"
          -H "Content-Type: application/json"
          -d '{"merge_request_iid": ${{ github.event.pull_request.number }},
               "source_branch": "${{ github.head_ref }}",
               "target_branch": "${{ github.base_ref }}",
               "head_sha": "${{ github.event.pull_request.head.sha }}"}'
```

Store `AI_PLATFORM_TOKEN` as a repository (or organization) Actions secret. The generated GitHub snippet does not set `continue-on-error`; a failed `curl` fails that job.

## After the POST

Areforge fetches the diff from the git host with the **repository access token** you gave at registration (not the CI token), reviews each file, and posts comments with that same access token. The CI token only authorizes the `POST`.

What happens next: [How a review works](./how-a-review-works.md).
