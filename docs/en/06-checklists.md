---
layout: default
title: 6. Operational checklists
parent: English
nav_order: 7
---

> 🌐 Version française : [6. Checklists](../06-checklists.md)

# 6. Operational checklists

## New project

- [ ] Idea and MVP scope defined.
- [ ] Visual inspiration selected.
- [ ] User flow validated.
- [ ] Master Prompt structured.
- [ ] `Enable Cloud` set to `Never allow` or `Ask each time`.
- [ ] External Supabase project created.
- [ ] Supabase organization authorized in Lovable.
- [ ] Supabase project connected to the Lovable project.
- [ ] GitHub connected.
- [ ] Migrations versioned.
- [ ] RLS enabled and audited.
- [ ] Auth URLs configured.
- [ ] Secrets server-side only.
- [ ] Tests with two users.

## Catch-up migration

- [ ] `.backup` export downloaded locally.
- [ ] Code synchronized to GitHub.
- [ ] Storage / Secrets / Jobs / Edge Functions inventory done.
- [ ] Target Supabase created.
- [ ] Schema restored.
- [ ] Dump format identified.
- [ ] Data restored with the appropriate tool.
- [ ] Tables and row counts verified.
- [ ] `auth.users` and `auth.identities` verified.
- [ ] Profile and creation trigger verified.
- [ ] RLS and policies verified.
- [ ] Site URL and Redirect URLs configured.
- [ ] Final export done before cutover.

## Switchover

- [ ] Maintenance window defined.
- [ ] Old backend frozen.
- [ ] Final export completed.
- [ ] Backups tested.
- [ ] GitHub `Connected`.
- [ ] Lovable Cloud removed only after validation.
- [ ] External Supabase connected.
- [ ] New Supabase variables applied.
- [ ] `supabase/config.toml` updated.
- [ ] App republished.
- [ ] Auth sign-in tested.
- [ ] Business CRUD tested.
- [ ] Multi-user isolation tested.
- [ ] Network requests verified.
- [ ] Supabase logs checked.

## Multi-LLM portability

- [ ] GitHub repo is the source of truth.
- [ ] `README.md` explains the architecture.
- [ ] `AGENTS.md`, `CLAUDE.md` or equivalent describes the conventions.
- [ ] `.env.example` without secrets.
- [ ] SQL migrations versioned.
- [ ] Local setup documented.
- [ ] Build, lint and tests reproducible.
- [ ] Minimal CI active.
- [ ] GitHub Pages documentation published.
- [ ] PDF or Word release archived at each major version.

## Security before public release

- [ ] No personal email visible in screenshots.
- [ ] No user UUID visible.
- [ ] No confidential client Project ID.
- [ ] No token, secret, password or PostgreSQL string.
- [ ] No client name without authorization.
- [ ] All examples use placeholders.
- [ ] Screenshots cropped or blurred.

---

<!-- nav-footer -->

<div align="center">

[← 5. Troubleshooting](05-troubleshooting.md) &nbsp;·&nbsp; [English home](../en.md) &nbsp;·&nbsp; [7. Multi-LLM portability →](07-multi-llm.md)

</div>
