---
title: FAQ
---

# FAQ

## Does our code leave our network?

The service runs on your own infrastructure. Diffs, comments, and history stay in your database. The only outbound call is to a language model, and choosing the gateway and the model is up to you. See [Data governance](./security.md).

## If AI writes the code and already knows our conventions, what is this service for?

Conventions and file structure can be respected at write time. The author is still not an independent reviewer. Logic bugs, security issues, and project-specific business rules are not visible the moment the code is written. Teams are not uniform — some people use Claude Code, some Cursor or Copilot, some write by hand; a tool-independent review at CI is the layer that makes them consistent. Enterprise and banking customers also need **proof that a review happened**, not just good code.

## Does it replace human reviewers?

No, and it is not meant to. The goal is a first-pass layer before the MR reaches a human, so senior engineers spend their attention on architecture and business logic rather than repetitive mechanical work.

## Does it work with GitHub?

Yes. GitLab and GitHub are both first-class paths; the CI snippet for each is generated when you register the repository. The primary path of the product is still self-hosted GitLab, because that is what most of the target teams run.

## Is there Persian output?

Yes. Review language is set at organization or repository scope, or in YAML. English is the default; Persian output works.

## How is runaway token cost prevented?

Several filter layers before anything reaches the model, plus optional per-file model selection: sensitive files get the strong model and tiny diffs get the cheap one. A retry does not re-spend tokens on a file that already produced a result. Real usage per run, broken down by model, is in the app dashboard. See [Cost and model routing](./configuration/cost-and-models.md).

## Do you support local payment and official invoices?

The commercial model is built for this market: payment in local currency with a domestic invoice. Self-serve checkout is not wired yet; access and billing are arranged with you directly.

## How long does setup take?

You register the repository, get a CI token and a ready snippet, and drop it into your pipeline. That path is designed to be self-service and under 15 minutes — once you have access to the service. [Quickstart](./getting-started.md).

## Is there an IDE extension or a CLI?

Not yet. Reviews run from CI on the merge request / pull request. Those surfaces are on the roadmap.

## Can a pull request turn the review down?

No. `.areforge.code-review.yaml` is read from the **base** branch. Custom guidance cannot override the reviewer’s hard principles.

## Are individual KPIs used for performance review?

No. Individual insights are formative and owner-only (plus the person themselves). They are not a ranking and they are not wired to pay. Team and organization dashboards stay at aggregate scope. See [Roles](./app/roles.md).
