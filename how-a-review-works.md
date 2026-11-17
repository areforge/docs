---
title: How a review works
---

# How a review works

A review starts when CI `POST`s the code-review endpoint. Everything after that is automatic.

## 1. CI sends the request

The pipeline posts the MR identifier, branches, and `head_sha` to `POST /api/v1/code-review` with the repository bearer token. It gets an immediate `202`. The work happens on the queue. Same head SHA again → `Already requested`; no second run.

Contract: [Connect CI](./ci.md).

## 2. Worthless files are dropped

Areforge fetches the diff from the git host with the **repository access token** (not the CI token), then partitions files:

- Deleted files
- Ignore-pattern matches (locks, minified, `vendor/`, `node_modules/`, `dist/`, `build/`, migrations, snaps, SVGs — plus anything you add)
- Include-pattern misses, when an allowlist is set
- Diffs below the minimum-lines threshold (default 8)
- Files whose rendered diff is larger than the per-file cap (default 1500 lines)

The remaining files are ordered **largest diff first**. If the run would exceed the instance cap (default 25 files), the overflow is skipped and flagged for manual review.

## 3. Every file is an independent review

Each remaining file is its own job in one batch, running in parallel. A failure in one file does not stop the others.

The model sees only the changed hunks, plus the MR title and a short description. Output is constrained to a structured schema: line, severity, category, body. Empty findings are allowed — filler comments are forbidden.

If the instance has per-file model selection enabled, sensitive paths get the strong tier, tiny non-sensitive diffs get the cheap tier, and everything else the default. Details: [Cost and model routing](./configuration/cost-and-models.md).

## 4. Comments on the exact line, plus a risk score

Each finding is posted as an inline discussion on the changed line. If the host rejects the inline position, Areforge falls back to a general note on that file and line.

At the end, one summary note on the MR:

- Files reviewed and comments posted
- Advisory **PR Risk** 0–100, with the top reasons
- Skipped files (collapsed)
- A sequence diagram, only when several components actually interact

The same run is stored in the app: the diff, the comments, token usage per file, and the risk score.

## A second push on the same MR

A new `head_sha` queues a **new** review. The same finding is not posted twice on that MR: comments carry a fingerprint, so a rephrased repeat is dropped. An interrupted run resumes without sending files that already produced a result back to the model.

## What you will not see

- Comments on formatting or style that a linter should catch
- Comments on unchanged lines
- A merge gate from PR Risk — the score is advisory
- A sequence diagram on a one-file or isolated change
