---
layout: default
title: 2. Preventing Lovable Cloud lock-in
parent: English
nav_order: 3
---

> 🌐 Version française : [2. Prévenir Lovable Cloud](../02-prevenir-lovable-cloud.md)

# 2. Preventing Lovable Cloud lock-in

## Why act before the first backend feature

Lovable Cloud can automatically provide database, Auth, Storage and functions.
Depending on the workspace configuration, the agent may enable Cloud when a request
needs a backend.

The Lovable documentation states that this behavior can be set in the Lovable Cloud
connector permissions, with three choices:

- **Always allow**: activation without confirmation;
- **Ask each time**: human confirmation on each need;
- **Never allow**: blocks Cloud activation.

For a project that must stay portable, choose **Never allow** or, at a minimum, **Ask
each time** before requesting a backend feature.

## Step A — Set the Lovable Cloud permissions

The exact wording may change. The documented path is close to:

```text
Workspace / Settings
> Connectors
> App connectors
> Lovable Cloud
> Manage permissions
```

Set **Enable Cloud** to:

```text
Never allow
```

or:

```text
Ask each time
```

This preference prevents automatic activation. It does not migrate a project already
attached to Lovable Cloud.

## Step B — Create the external Supabase

In Supabase:

1. create an organization or use an existing one;
2. create a project in the right region;
3. keep the PostgreSQL password in a password manager;
4. note the Project ID and Project URL;
5. never publish the `service_role` or the secret key.

## Step C — Connect Supabase to Lovable

The official path is generally:

```text
Project Settings
> Integrations
> Supabase
> Connect Supabase
```

Depending on the UI version, the connection may also be reachable from the connectors
menu.

After authorization:

1. choose the Supabase organization;
2. select the existing project;
3. verify the Project ID matches the expected project;
4. do not create a new project if the target project already exists.

## Step D — Connect GitHub

In Lovable:

```text
Project Settings
> Git
> Connect GitHub
```

Check:

- the right account or organization;
- the `main` branch;
- the `Connected` status;
- the creation of the expected repository;
- the absence of secrets in the repository.

Once connected, the repository becomes the reference for the code and lets you work
with other tools.

## Step E — Configure the backend before generating it

Before asking "add authentication" or "create the database", prepare:

1. the required Auth providers;
2. the Site URL and Redirect URLs;
3. the `user_id` convention;
4. the expected RLS policies;
5. the `supabase/migrations/` folder;
6. server-side secrets;
7. multi-user isolation tests.

## Safeguard prompt

```text
CONSTRAINTS — BACKEND OWNERSHIP

- Do not enable or create Lovable Cloud.
- Use only the externally connected Supabase project.
- Store every schema change as a versioned SQL migration.
- Use only the public/publishable Supabase key in frontend code.
- Never expose service_role, secret keys, database passwords or API secrets.
- Enable and validate RLS on every exposed public table.
- Keep the project synchronized with GitHub.
- Present any destructive migration before execution.
```

## Exit check

The new project is ready when:

- Lovable Cloud cannot activate automatically;
- GitHub is connected;
- Supabase is connected;
- the Project ID is verified;
- the frontend variables are public;
- the secrets are stored server-side;
- migrations will be versioned.

---

<!-- nav-footer -->

<div align="center">

[← 1. The Gab-E OPEN method](01-method-open.md) &nbsp;·&nbsp; [English home](../en.md) &nbsp;·&nbsp; [3. Catch-up migration →](03-migration.md)

</div>
