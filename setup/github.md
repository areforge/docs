---
title: GitHub
---

# Connect GitHub

GitHub is a first-class path. The primary product path is still self-hosted GitLab, because that is what most of the target teams run — GitHub is fully supported.

## Register the repository

In the app: **Add repository** → GitHub.

| Field | Value |
|---|---|
| Repository identifier | `owner/repo` |
| Access token | A token that can read the repository and post review comments on pull requests |
| Stack profile | Optional. See [Stack profiles](../configuration/stack-profiles.md) |
| Ownership | Personal, or a team you already belong to |

On success, save the **CI token** (shown once) and paste the generated workflow under `.github/workflows/`. Full contract: [Connect CI](../ci.md).

## Access token

A classic personal access token with `repo` (private repositories) can read the code and post pull-request comments.

A fine-grained token for that repository needs at least:

- **Contents:** Read
- **Pull requests:** Read and write

Areforge stores the token encrypted and never returns it in JSON. If GitHub revokes it, replace it from [Credentials](./credentials.md).

## Actions secret

Store `AI_PLATFORM_TOKEN` as a repository (or organization) Actions secret. The generated snippet reads that name.

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

Use the snippet from the app, not this example. The generated GitHub snippet does not set `continue-on-error`; a failed `curl` fails that job.

**`head_sha` must be `github.event.pull_request.head.sha`.** On a `pull_request` event, `github.sha` is the throwaway merge commit, not the branch head. Areforge keys a run by that SHA; the wrong one either duplicates or never matches the comments.

## After the POST

Areforge fetches the pull request and the diff with the **access token**, reviews each file, and posts comments with that same token. The CI token only authorizes the `POST`.
