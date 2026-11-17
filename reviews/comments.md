---
title: Comments and findings
---

# Comments and findings

Each reviewed file produces zero or more **findings**. A finding is posted as an inline discussion on the changed line. If the git host rejects that position, Areforge falls back to a general note naming the file and line.

## Shape

Every finding has:

| Field | Meaning |
|---|---|
| `line` | Line number in the new file (a changed line, not context) |
| `severity` | `critical`, `warning`, `suggestion`, or `nit` |
| `category` | One tag from a closed vocabulary |
| `body` | Why it is a problem, and a one-sentence direction to fix it |

Empty findings are allowed. The reviewer is instructed not to invent a comment just to say something, and not to flag formatting that a linter should catch.

## Severity

| Value | Use |
|---|---|
| `critical` | Bug, security issue, data loss |
| `warning` | Serious design or performance issue that should be fixed before merge |
| `suggestion` | A real improvement that is not required to merge |
| `nit` | Minor point |

The [review profile](../configuration.md#profiles) changes how many of these are posted. `quiet` drops `suggestion` and `nit` after the model runs. `balanced` and `thorough` keep every real finding.

## Categories

The agent must tag each finding with one of:

`security`, `auth`, `payment`, `transaction`, `injection`, `sql-injection`, `performance`, `n+1`, `concurrency`, `error-handling`, `validation`, `data-loss`, `testing`, `design`, `readability`, `naming`, `a11y`, `other`.

`other` is the fallback when nothing specific applies. Analytics, acceptance-by-category, and the PR Risk security signal all group on this exact string.

## Language

Comment language is a free-form prompt string (max 50 characters), not a locale enum. English is the default. Persian works. Set it at organization or repository scope, or in [`.areforge.code-review.yaml`](../configuration.md).

## On the merge request

The discussion body is the finding, plus:

- A recurrence note when [memory](./memory.md) recognised the same issue on an earlier MR
- A hidden fingerprint so a later run on the same MR does not post the same finding twice, even if the model rephrases it

Resolving the discussion is how Areforge records **acceptance**. There is no thumbs-up / thumbs-down reaction yet. Acceptance KPIs count only on **merged** MRs. See [Insights](../app/insights.md).

## In the app

Repository → **Reviews** → a merge request. You get the stored diff, comments overlaid on the changed lines, skipped-file reasons, and the sequence diagram when one was generated. While a review is running, the page refreshes itself.
