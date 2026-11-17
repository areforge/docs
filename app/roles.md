---
title: Roles
---

# Roles

Four system roles, assignable at **team** or **repository** scope. Permission sets are cumulative: reporter ⊃ member, maintainer ⊃ reporter, owner ⊃ maintainer.

Effective access to a repository is the **union** of:

1. Ownership (personal owner, or owner of the organization that owns the team)
2. Direct repository membership
3. Inherited team membership

The highest permission wins. A direct `member` role cannot silently demote a team `maintainer`.

Owner is not an invitation target. You become owner by creating the organization or the personal repository.

## What each role can do

| | Owner | Maintainer | Reporter | Member |
|---|:---:|:---:|:---:|:---:|
| View the repository and its reviews | ✓ | ✓ | ✓ | ✓ |
| View own insights | ✓ | ✓ | ✓ | ✓ |
| View members, settings, OKRs, repo/team KPIs | ✓ | ✓ | ✓ | |
| Create / update repository, invite / remove members, manage settings and credentials, manage OKRs | ✓ | ✓ | | |
| Delete repository, change member roles | ✓ | | | |
| View **other members’** individual insights | ✓ | | | |

Member is the default minimal seat: they see the repository and their own growth, not the dashboard rollup.

## Individual data

`kpi.view.members` / `reviews.view.members` belong **only to owner**. That is a product line, not an accident: individual comments exist for mentoring, not for ranking or pay. See [Members](./members.md).

## Organization

An organization currently has a single owner. There is no organization-wide membership table. Team roles under the organization are how other people get in.
