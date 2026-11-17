---
title: Review configuration
---

# Review configuration

Review behaviour comes from four layers. The most specific layer that sets a field wins, except for a few **union** keys that add up instead of replacing.

1. `.areforge.code-review.yaml` on the merge-request **base** branch
2. Repository settings in the app
3. Organization settings in the app (team-owned repositories)
4. Schema defaults

A personal repository has no organization layer.

The YAML file is an override **on top of** the dashboard, not a replacement for it. Stack profile is chosen when you [register the repository](./getting-started.md); it is not a YAML field. See [Stack profiles](./configuration/stack-profiles.md).

## `.areforge.code-review.yaml`

Put this file at the **repository root**. Areforge reads it from the MR’s **target / base** branch, never from the head branch. A pull request cannot weaken its own review by editing this file on the feature branch.

The file is fail-soft. A missing file, invalid YAML, or a bad field never fails the review. Unknown keys are ignored. One invalid field is dropped on its own; the rest of the file still applies. If an array-shaped field has one bad item, that **whole field** is dropped (not just the bad item).

Only the `review:` block is read.

```yaml
review:
  language: Persian
  profile: balanced
  sequence_diagrams: true
  custom_guidance: |
    Prefer Form Requests over inline validation.
    Flag N+1 queries in controllers.
  ignore_patterns:
    - 'generated/**'
  path_instructions:
    - path: 'app/Http/Controllers/**'
      instructions: Flag missing authorization on every write action.
  min_lines_threshold: 8
```

`language` is a free-form prompt string (max 50 characters), not a locale enum. English is the default. Persian works.

## Fields

| Field | Default | Merge |
|---|---|---|
| `language` | `English` | replace |
| `profile` | `balanced` | replace |
| `sequence_diagrams` | `true` | replace |
| `include_patterns` | `[]` (review everything) | replace — a non-empty allowlist **narrows** the parent |
| `ignore_patterns` | locks, minified, `vendor/`, `node_modules/`, `dist/`, `build/`, migrations, snaps, SVGs | **union** |
| `sensitive_patterns` | Auth, Security, payment, billing, transaction globs | **union** (security floor; you can add, not remove) |
| `test_patterns` | `*/tests/*`, `*Test.php`, `*.test.*`, `*.spec.*`, `*_test.*` | **union** |
| `path_instructions` | `[]` | **union** (org, then repo, then YAML) |
| `custom_guidance` | empty | **union** (org, then repo, then YAML, joined with a blank line) |
| `min_lines_threshold` | `8` | replace — skip a file whose diff is smaller than this |
| `max_total_lines_per_file` | `1500` | replace |
| `trivial_max_changed_lines` | `10` | replace — cheap model tier for a small, non-sensitive diff |
| `large_change_lines` | `300` | replace — PR Risk “large change” signal |

`path_instructions` items are `{path, instructions}`. `path` is a glob matched with `fnmatch` against the file path. `instructions` is at most 2000 characters.

Custom guidance and path instructions cannot override the reviewer’s hard principles (for example by asking it to downplay a real security issue).

## Profiles

`profile` only changes how many **real** findings are posted. It never licenses filler.

| Value | Effect |
|---|---|
| `quiet` | Critical and warning only. Suggestion and nit are dropped after the model runs. |
| `balanced` | Every real finding at its honest severity. Default. |
| `thorough` | Also surface well-founded suggestions and nits. Still no invented comments. |

## What YAML cannot set

These stay at the schema default so a repository cannot uncap spend or fragment the dashboard rollup:

- Max files reviewed per run (`25`)
- High-risk PR Risk threshold (`60`)

Sequence-diagram **volume** (at least two reviewed files and a minimum changed-line total) is an instance tuning knob, not a per-repo setting. See [Sequence diagrams and the system map](./system-map.md).

## Dashboard vs YAML

The app review-settings screen shows **own** (set at this scope) and **effective** (resolved from repo → org → default). Those effective values **do not include** `.areforge.code-review.yaml`. The file only applies when a review actually runs.

If the live review disagrees with the screen, check the YAML on the **base** branch.

## In the app

Repository or organization → review settings. The same field names as the YAML `review:` block. A `PATCH` sends only the keys you change; omitted keys stay as they are. Unchecking a field returns it to the inherited value.

A personal repository has no organization layer to inherit from.

Token cost and model tiers: [Cost and model routing](./configuration/cost-and-models.md).
