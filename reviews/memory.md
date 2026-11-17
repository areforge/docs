---
title: Memory
---

# Memory

If Areforge has already reported the same finding on an earlier merge request in this repository, it still posts the comment and attaches a **“said this before”** note, with the originating MR. It never suppresses. The bug may still be open.

Memory is **per repository**. One repository does not learn from another.

## Two levels

| Level | What matches | Typical note |
|---|---|---|
| File | Same file, same category, same wording (line numbers may have shifted) | Previously reported in this file on MR !214 |
| Pattern | Same category and wording anywhere in the repository | This anti-pattern has come up before in the repository |

File-level is preferred when both match.

Wording is normalized: case, punctuation, and numbers are ignored, so a rephrase of the same issue still collides. The match is not a fuzzy semantic search.

## Dedup on the same MR

A second run on the **same** merge request (a new push, or a retry) does not post the same finding twice. Comments carry a fingerprint in a hidden HTML marker, so dedup still works if the local database is rebuilt from the git host.

That is a different mechanism from memory. Memory is across MRs. Dedup is inside one MR.

## In the app

Reviews and individual insights tag recurring findings. Repository KPIs show **repeat findings** as a share of all findings, and [objectives](../app/objectives.md) can target `recurring_findings_percent`.
