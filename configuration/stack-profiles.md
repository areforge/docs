---
title: Stack profiles
---

# Stack profiles

A stack profile adds **stack-specific guidance** to the reviewer: N+1 and Form Requests on Laravel, for example. It is chosen when you [register the repository](../getting-started.md), and can be changed later on the repository settings screen. It is **not** a YAML field.

You can connect a repository without a profile. The reviewer then uses its general principles and whatever custom guidance you set.

## Registered profiles

| Key | Label |
|---|---|
| `laravel` | Laravel / PHP |
| `nodejs` | Node.js |
| `nextjs` | Next.js / React |
| `vue` | Vue |
| `dotnet` | .NET |
| `go` | Go |
| `wordpress` | WordPress |
| `django` | Django |
| `backend-generic` | Backend (generic) |
| `frontend-generic` | Frontend (generic) |

The list in the app comes from `GET /api/v1/repositories/stack-profiles`. Adding a stack is a new class on the instance; it does not require a docs or app release for the dropdown to pick it up.

## What a profile does

It is extra prompt text, applied **together with** the global review principles. It does not replace them. A Laravel profile still cannot ask the model to ignore a real security finding.

Typical Laravel attention:

- N+1 queries and missing eager loading
- Form Requests instead of scattered controller validation
- Transactions and race conditions on payments / inventory
- Mass assignment (`$fillable` / `$guarded`)
- Secrets in API responses, logs, or production stack traces

Other stacks follow the same idea for their own failure modes.

## Custom guidance vs a profile

| | Stack profile | Custom guidance / path instructions |
|---|---|---|
| When | At register, or repository settings | Review settings or YAML |
| Scope | Whole repository | Whole repository, or a glob |
| Who writes it | Areforge | You |

Use a profile for the framework. Use custom guidance for house rules (“flag missing authorization on every write action”).
