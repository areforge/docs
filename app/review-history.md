---
title: Review history
---

# Review history

Every merge request Areforge has reviewed on a repository, newest first. Path: `/{locale}/repositories/{identifier}/reviews`.

## List

Each row is a review request: MR number, branches, status, duration, risk badge, recurring / accepted tags when they apply.

Statuses: **Queued**, **Running**, **Completed**, **Failed**, **Skipped**.

While a review is running, the page refreshes itself.

Empty is normal until the first CI `POST` succeeds. If it has been a while: the snippet is at the repository root, and `AI_PLATFORM_TOKEN` is defined. See [Troubleshooting](../troubleshooting.md).

## One merge request

`/{locale}/repositories/{identifier}/reviews/{mr}` is the timeline of every review on that MR (oldest first), because a second push creates a new request.

Open a run to see:

- The **stored diff** — exactly what the model reviewed, comments overlaid on the line
- File-level comments (when the host rejected an inline position)
- **Skipped files** and the reason (ignore pattern, below threshold, too large, over the file cap)
- **Sequence diagram** when one was generated
- **PR Risk** and the factor list
- The summary note text

A stale banner appears when this is not the latest review (the branch was pushed to after it). Jump to the latest from there.

The diff is stored at review time. Areforge does not re-fetch it from the git host to render this page. If a file’s text was not stored, the comments are still listed.

## Link back to the host

Each review can open the merge request or pull request on GitLab or GitHub.
