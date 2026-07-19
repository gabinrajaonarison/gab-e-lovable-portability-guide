---
layout: default
title: 7. Multi-LLM portability
parent: English
nav_order: 8
---

> 🌐 Version française : [7. Portabilité multi-LLM](../07-portabilite-multi-llm.md)

# 7. Moving from Lovable to Claude, ChatGPT, Codex, Cursor or a local IDE

## The principle

Once GitHub and Supabase are externalized, Lovable is no longer the only environment
able to evolve the project.

The same repository can be used by:

- Lovable;
- Claude Code or Claude Desktop with a development environment;
- ChatGPT / Codex;
- Cursor;
- VS Code;
- a team of developers;
- an external CI/CD.

## GitHub becomes the shared contract

The repository should contain:

```text
README.md
AGENTS.md or CLAUDE.md
src/
supabase/migrations/
.env.example
package.json
test scripts
```

Each AI agent should receive the same rules:

- architecture;
- conventions;
- commands;
- scope;
- security constraints;
- prohibition of destructive migrations;
- success criteria.

## Example `AGENTS.md` file

```markdown
# Project rules

## Architecture
- Frontend: React / TypeScript.
- Backend: external Supabase project.
- Database schema: versioned in supabase/migrations.

## Security
- Never expose service_role or database passwords.
- All public tables require RLS.
- Never modify auth schema directly without an approved migration plan.

## Change policy
- Present destructive changes before execution.
- Do not rename tables or columns outside the requested scope.
- Run build and tests before completion.
```

## Clone and work locally

```bash
git clone https://github.com/OWNER/REPOSITORY.git
cd REPOSITORY
npm install
cp .env.example .env.local
npm run dev
```

The real values in `.env.local` stay out of Git.

## Handover prompt to another agent

```text
CONTEXT
This project was initially built with Lovable, but GitHub is now the source of truth and the backend is an external Supabase.

TASK
Implement the requested feature in the existing repository.

GUIDELINES
Respect the architecture, versioned migrations, RLS policies and the conventions described in AGENTS.md.
Run build, lint and tests.

CONSTRAINTS
Do not modify the secrets.
Do not create a new backend.
Do not replace Supabase.
Do not run a destructive migration without validation.
Do not touch files outside the scope.
```

## Deploy outside Lovable

The frontend can then be deployed to:

- Vercel;
- Netlify;
- Cloudflare Pages;
- a VPS;
- a container;
- enterprise infrastructure.

The Supabase backend stays the same as long as the environment variables point to the
same project.

## Recommended strategy

1. Lovable to speed up prototyping and UI iterations.
2. GitHub as the source of truth.
3. External Supabase as the controlled backend.
4. Claude or ChatGPT for audits, refactors and targeted tasks.
5. CI/CD to validate build, lint and tests.
6. GitHub Pages to document the method and the decisions.

---

<!-- nav-footer -->

<div align="center">

[← 6. Operational checklists](06-checklists.md) &nbsp;·&nbsp; [English home](../en.md) &nbsp;·&nbsp; [8. Case study: a CRM project →](08-case-crm.md)

</div>
