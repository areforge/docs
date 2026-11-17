---
title: Organizations and teams
---

# Organizations and teams

Areforge is multi-tenant. A user can own personal repositories with no organization at all. Sharing work uses this tree:

```text
User ──owns──> Organization ──has──> Team ──has──> Repository
User ──owns──> Repository (personal)
```

The app nav is **Home**, **Repositories**, **Organizations**. Account lives in the user menu.

## Organizations

`/{locale}/organizations`. Create an organization, rename it, or delete it.

Delete is a danger-zone action: every team and repository under it becomes unreachable for its members. Review history is kept. Only the organization owner can do this.

An organization has **no membership table of its own**. Access to organization-level insights is through owning the organization, or through teams under it.

## Teams

Inside an organization: create a team, rename it, delete it, invite members.

A team is the unit you attach a **team repository** to. When you add a repository, **Ownership** is Personal or a team you already belong to. The team **cannot be changed after create**.

Deleting a team drops members’ access to its repositories.

## Personal vs team repositories

| | Personal | Team |
|---|---|---|
| Who sees it | You (and anyone you invite directly) | The team, plus anyone invited directly |
| Organization settings | None — no org layer to inherit | Review settings inherit organization → repository |
| Insights rollup | Repository only | Repository, team, and organization |

Direct repository members are listed separately from the team. See [Members and invitations](./members.md).

## Insights at each scope

- Repository → **Dashboard**
- Team → team metrics (every repository that team owns, rolled up on the server)
- Organization → organization metrics, then **teams side by side** (ordered by how much was reviewed, not by score)

Details: [Insights](./insights.md).
