---
title: Members and invitations
---

# Members and invitations

People get access in two ways: **team membership** (inherited by every repository on that team) and **direct repository membership**.

## Invite

On a team or a repository, **Invite member**. The invitation goes to an **email address**. They have no access until they open the link and accept.

Invitable roles: **maintainer**, **reporter**, **member**. Owner is implicit — the organization owner or the personal-repository owner — and is not invited. See [Roles](./roles.md).

Pending invitations show an expiry. **Revoke** kills the link; you can invite the same address again.

## Accept

The link lands on `/{locale}/invitations/{token}`.

1. Sign in first. The invitation attaches to the account you are signed in with.
2. That account’s **verified email** must match the invitation. Add and confirm email from [Account](./account.md) if you have not.
3. Accept. You are then on the members list.

A mismatched email, an expired link, or a revoked invitation cannot be used. Ask whoever invited you to send a new one.

## Change role and remove

Owners (and, for invite/remove, maintainers) can change a member’s role and remove them.

- Remove from a **team** — they lose access to every repository in that team.
- Remove from a **repository** — they lose direct access to that repository. Team membership, if they still have it, is untouched.

## Individual insights

On a repository members list, an owner can open **View insights** for that person: comments on their code in this repository, findings per MR, recent comments.

This is **formative, not a ranking**. “40 critical comments” is not a worse number than “10” — it depends on how much was written and what kind of code it was. Use the comments to help. The same framing is on **My Growth** / Account insights for your own code.

Only the **owner** role sees other people’s individual data by default. Maintainers, reporters, and members do not.
