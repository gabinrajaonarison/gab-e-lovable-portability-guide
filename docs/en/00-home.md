---
layout: default
title: Home
parent: English
nav_order: 1
---

> 🌐 Version française : [Accueil](../index.md)

# The Gab-E OPEN Method (KISS-free)

## Build fast with Lovable, without losing control of your code or backend

Lovable is a great accelerator to turn an idea into a working app. The risk appears
when the code, database, authentication, storage and secrets become hard to move to
another environment.

This guide formalizes one simple rule:

> **Lovable should stay a build accelerator, not the single point of control of your
> project.**

The target foundation is:

```text
Idea + User flow + Master Prompt
              |
              v
        Lovable AI Agent
              |
              | generates and syncs the code
              v
GitHub = source of truth for the code
              |
              +------> Claude / ChatGPT / Codex / Cursor / local IDE
              |
              v
Supabase (external) = PostgreSQL + Auth + Storage + Functions
              |
              v
Lovable Publish / Vercel / Netlify / other hosting
```

## Two tracks

### Track A — Preventive

For a new project:

1. set the Lovable Cloud permissions;
2. connect GitHub;
3. create and connect your own Supabase;
4. only then ask Lovable for Auth, database or storage.

### Track B — Catch-up

When a project is already on Lovable Cloud:

1. inventory and export;
2. restore into a Supabase you control;
3. repair the incomplete parts;
4. verify Auth, profiles, triggers and RLS;
5. connect GitHub;
6. switch the app over;
7. remove Lovable Cloud only after validation.

## Warning

Backend deletions, PostgreSQL restores and Auth changes can be irreversible. Work
first on a fresh project or a test environment, keep several backups, and never copy
secrets into a prompt or a public repository.

---

<!-- nav-footer -->

<div align="center">

[English home](../en.md) &nbsp;·&nbsp; [1. The Gab-E OPEN method →](01-method-open.md)

</div>
