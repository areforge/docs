---
title: Sequence diagrams and the system map
---

# Sequence diagrams and the system map

Two related pictures:

- **This MR.** When the change is a real interaction across components, Areforge posts a Mermaid **sequence diagram** on that merge request.
- **This repository.** Those diagrams merge, over time, into a **system map** — a flowchart of the architecture the reviews have actually seen.

The sequence is a walkthrough of one change. The map is the big picture: how the codebase is wired, built from the MRs rather than from a hand-drawn architecture slide. Developers use it to review and onboard. Product uses it as observed architecture — the paths that keep showing up, not the paths a diagram once claimed.

## When a sequence diagram is posted

All of these have to hold:

1. `sequence_diagrams` is enabled (default **on**; disable in [review settings or YAML](./configuration.md)).
2. At least **two files** were reviewed on this MR.
3. Total changed lines meet the instance threshold (default **20**). A one-file change never qualifies.
4. The combined diff is under the same input-size cap as a per-file review.
5. The diagram agent sets `shouldGenerate: true` — the files represent a meaningful call chain, event/job flow, or API path. Isolated copy, config, or a self-contained function does not get a diagram.

If any check fails, the review still completes. There is simply no diagram on that MR.

The diagram is valid Mermaid `sequenceDiagram` syntax, posted on the MR summary (collapsed) and stored with that review. Participants are limited to actors evidenced by the diff (about eight at most).

## How the system map is built

Each generated sequence is parsed and folded into the repository graph. No extra model call: participants become nodes, messages become edges.

- Nodes match **by name, case-insensitively**. `OrderService` and `orderService` collapse; `Order Service` and `Orders` do not. One component written two ways can appear as two nodes.
- An edge’s **`occurrence_count`** is how many merge requests saw that path. That count is the hot-path vs seen-once signal the picture cannot show; the app lists it under the diagram.
- The aggregate is rendered as Mermaid **`flowchart TD`**, not `sequenceDiagram`. Merged flows have no single timeline; drawing them as one sequence would invent an order the data does not have.

A merge failure is fail-soft. The MR still gets its own sequence; the repository map is left as it was.

Turning `sequence_diagrams` off does not remove diagrams already made; no new ones are produced.

## Empty is normal

A newly connected repository, or one whose MRs have been small or single-file, has no map. The app empty state is that fact, not an error.

## In the app

Open the repository → **System map**. The page is `/{locale}/repositories/{identifier}/system-map`. It loads once per visit (the graph only changes when a new diagram merges).

Under the flowchart: **Most frequent interactions** — the same edges, ordered by `occurrence_count`.
