---
title: GitLab
---

# Connect GitLab

GitLab is a first-class path, including self-hosted GitLab.

## Register the repository

In the app: **Add repository** → GitLab.

| Field | Value |
|---|---|
| Repository identifier | Numeric project ID, or the full path such as `group/repo` |
| Access token | A token that can read the project and post notes / discussions on merge requests |
| Server address | Optional. Set this for self-hosted GitLab (`https://gitlab.example.com`). Leave empty for gitlab.com |
| Stack profile | Optional. See [Stack profiles](../configuration/stack-profiles.md) |
| Ownership | Personal, or a team you already belong to |

On success, save the **CI token** (shown once) and paste the generated job into `.gitlab-ci.yml`. Full contract: [Connect CI](../ci.md).

## Access token

Use a **project access token** or a personal access token with enough scope to:

- Read the repository and merge-request diffs
- List discussions on the merge request
- Post inline discussions and notes

On GitLab, the `api` scope covers that. Restrict the token to this project when you can. Areforge stores it encrypted and never returns it in JSON.

If the token is revoked at GitLab, reviews stop. Replace it from the repository’s [Credentials](./credentials.md) screen; you do not recreate the repository.

## CI variable

Define `AI_PLATFORM_TOKEN` as a **masked** CI/CD variable on that project (protected if your host supports it). The generated snippet reads that name.

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

Use the snippet from the app, not this example: the URL and `X-Repository-Id` must be the ones issued for **this** repository. `allow_failure: true` so a review outage does not fail the pipeline.

## After the POST

Areforge fetches the MR, the diff, and existing discussions with the **access token**, reviews each file, and posts comments with that same token. The CI token only authorizes the `POST`.
