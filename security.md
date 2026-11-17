---
title: Data governance
---

# Data governance

For banks, government bodies, and any team that cannot hand its business logic to a foreign SaaS, this is the core difference.

## Where code lives

The service runs on the infrastructure and database you already operate. Diffs, comments, and history stay in your database. The only outbound call is to the **model gateway you configured**.

## Tokens

| Secret | Storage | Shown again? |
|---|---|---|
| Git-host access token | Encrypted at rest, stripped from every JSON response | Never |
| CI token | Hashed at rest | Only at create or rotation, once |

Each CI token is scoped to exactly one repository. Rotation is on the [Credentials](./setup/credentials.md) screen.

## The reviewer cannot be tool-injected

The code-review agent has **no tools** and is limited to a **single step**. Even if a diff or MR description carries a malicious instruction, the model can only return structured JSON findings. Custom guidance cannot override the hard principles (for example by asking it to ignore a real security issue).

YAML is read from the **base** branch, so a pull request cannot weaken its own review.

## Rate limit

The model-call limit is shared across workers, so one large MR cannot put your model account under pressure. The per-run file cap (default 25) is instance-wide for the same reason.

## Audit trail

Every request, file run, and comment is recorded. The app review page is the stored diff plus the comments that were posted — proof that the review happened, not only better code.

## Access control

See [Roles](./app/roles.md). Individual developer insights are owner-only and formative. They are not a ranking and they are not wired to pay.

## What is not here yet

Self-serve billing, step-up OTP for credential rotation, and an audit log UI for membership changes. Access and billing are arranged with you.
