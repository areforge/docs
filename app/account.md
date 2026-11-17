---
title: Account
---

# Account

The signed-in person. Path: `/{locale}/account`. It is in the user menu, not the main nav.

## Profile

- Name
- Mobile number (the OTP identity; this is how you sign in)
- Email, with verified / pending
- Status (active, inactive, pending approval, blocked)

**Add email** or **Change email** sends a confirmation link. You need a **verified** email to accept [invitations](./members.md).

## Sign in

Login is OTP to an 11-digit mobile number in the form `09xxxxxxxxx`. The first successful OTP **creates** the account. There is no password.

## My reviews

Account → insights (`/{locale}/account/insights`) is **My Growth**: comments on your own code, findings per MR, recent comments.

This page is yours alone. Counts are a way into the comments, not a score. Home shows a compact version of the same metrics.

## Sign out

From the user menu. The session is a bearer token in the app; signing out revokes it.
