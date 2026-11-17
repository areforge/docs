---
title: Credentials
---

# Credentials

Two keys make a repository work. Neither is readable again once it is set.

| Key | Who uses it | What it does |
|---|---|---|
| **Repository access token** | Areforge → GitLab / GitHub | Reads the diff and posts comments |
| **CI token** | Your pipeline → Areforge | Authorizes `POST /api/v1/code-review` |

Open the repository → **Settings**. The **Credentials** panel is there. Only an owner or maintainer can change these keys. See [Roles](../app/roles.md).

## Repository access token

Code is read and comments are posted with this token. If it is revoked at the git host, reviews stop without a loud failure on the MR — the next run cannot fetch the diff.

The panel shows **Working**, **Not working**, or **Checking**. A background check runs after you save a token; you can also press **Check again** if you have fixed access at the git host and do not need to retype anything.

**Replace token** stores the new value encrypted and invalidates the previous one immediately. Areforge never shows the token again.

## CI token

Your pipeline requests reviews with this token. It is hashed at rest. The plaintext is shown **exactly once**: at repository create, and again if you issue a new one.

**Issue a new token** invalidates the current value immediately. Until you put the new one into `AI_PLATFORM_TOKEN` (GitLab CI/CD variable or GitHub Actions secret), no new review is recorded.

The generated snippet still uses the same variable name and `X-Repository-Id`. Only the secret value changes.

## If you lost the CI token

You do not recreate the repository. Issue a new CI token from this screen, update the CI variable, and the next MR pipeline is enough.
