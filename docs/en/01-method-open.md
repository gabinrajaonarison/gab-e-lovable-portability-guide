---
layout: default
title: 1. The Gab-E OPEN method
parent: English
nav_order: 2
---

> 🌐 Version française : [1. Méthode Gab-E OPEN](../01-methode-gab-e-open.md)

# 1. The Gab-E OPEN Method

## What OPEN means

**Gab-E OPEN** is four requirements to hold from the very start:

- **O — Ownership**: you own your code (GitHub) and your data (Supabase).
- **P — Portability**: the project can move to another tool, assistant or host.
- **É — Ecosystem**: knowledge and building blocks stay open, shareable, reusable.
- **N — Neutrality**: no single tool becomes the only point of control of the project.

It is the same spirit as Gab-E's circular economy: nothing gets locked in, everything
can be recovered, reused and passed on.

## The original method

The Gab-E OPEN method aims to move quickly from an idea to a first testable product:

1. **Start with an Idea** — about 5 minutes.
2. **Find Design Inspiration** — about 5 minutes on Dribbble or a similar source: shapes, colors, density, components.
3. **Design the User Flow** — about 30 minutes on Whimsical, Miro or a similar tool, staying focused on the core value proposition.
4. **Build the Master Prompt** — manually or with a Gab-E Master Prompt.
5. **Give the Master Prompt to Lovable** — attach the user flow, visual references and theme.
6. **Let Lovable build it out.**
7. **Test, iterate, repeat.** Don't be afraid to break things in a controlled environment.

## The added safeguard: Ownership First

The problem was never Lovable's speed, but the lack of an explicit technical-ownership
step.

The reinforced method therefore adds a **portability gate** before any backend request:

```text
Before Auth / Database / Storage:

1. Choose the Lovable Cloud policy.
2. Connect GitHub.
3. Create and connect the external Supabase.
4. Prepare variables and migrations.
5. Only then let Lovable build the backend.
```

## Reinforced method in eight steps

1. **Idea**: problem, target, expected outcome.
2. **Inspiration**: style, components, visual hierarchy.
3. **User flow**: main path, exceptions and empty states.
4. **Master Prompt**: context, task, guidelines and constraints.
5. **Ownership Gate**: GitHub, Supabase, Cloud permissions, secrets.
6. **Lovable build**: frontend and backend wired to the controlled foundation.
7. **Validation**: Auth, CRUD, RLS, errors, responsive, performance.
8. **Multi-LLM iteration**: Lovable, Claude, ChatGPT, Cursor or local IDE from the same repository.

## Assets to control

| Asset | Recommended source of truth |
|---|---|
| Code | GitHub |
| Database schema | `supabase/migrations/` |
| Data | Supabase + controlled backups |
| Auth | External Supabase |
| Files | Supabase Storage or external storage |
| Secrets | Server-side secret manager |
| Documentation | GitHub Pages |
| Structuring prompts | GitHub repo or versioned knowledge base |

## Non-negotiable principles

- GitHub is connected early.
- The external Supabase is connected before the first backend feature.
- SQL migrations are versioned.
- The frontend only ever receives public keys.
- Server secrets never start with `VITE_`.
- RLS is enabled and tested with at least two accounts.
- The `.env.example` file contains no real sensitive value.
- The exit procedure is documented before you need it.
- Lovable Cloud is deleted only after export, restore and tests.

## Recommended Master Prompt structure

```text
CONTEXT
Locate the project, page, flow and target infrastructure.

TASK
Describe precisely the result to produce.

GUIDELINES
Specify the stack, conventions, migrations, Auth, RLS and tests.

CONSTRAINTS
Forbid enabling Lovable Cloud, exposing secrets, out-of-scope changes and unvalidated destructive migrations.
```

---

<!-- nav-footer -->

<div align="center">

[← Home](00-home.md) &nbsp;·&nbsp; [English home](../en.md) &nbsp;·&nbsp; [2. Preventing Lovable Cloud lock-in →](02-prevent-cloud.md)

</div>
