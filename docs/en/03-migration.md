---
layout: default
title: 3. Catch-up migration
parent: English
nav_order: 4
---

> 🌐 Version française : [3. Migration de rattrapage](../03-migration-rattrapage.md)

# 3. Catch-up migration: Lovable Cloud to your Supabase

This track applies when a project already uses Lovable Cloud and must be taken over
into a Supabase project controlled by its owner.

## 3.1 Freeze and inventory

Before any irreversible action:

- temporarily stop functional changes;
- note the tables, enums, functions, triggers and policies;
- list Auth, providers and users;
- list Storage buckets and files;
- list Edge Functions, Jobs and Secrets;
- note the public URL and the Auth URLs;
- connect GitHub if not already done.

## 3.2 Export Lovable Cloud

The Lovable documentation now allows exporting the database from:

```text
Cloud
> Overview
> Advanced settings
> Export project data
```

Exporting the database and downloading Storage files are two separate operations.

Keep the dump:

- on your computer;
- in an encrypted backup;
- outside the GitHub repository.

## 3.3 Create the target Supabase

Create an empty project in your own organization and record:

```text
Project ID
Project URL
Database password
Region
Publishable key
```

Never share the database password, the `service_role` or the secret key.

## 3.4 Replay the schema

The most traceable method is to run the migrations from:

```text
supabase/migrations/
```

in chronological order.

A consolidated SQL script can be used when it faithfully represents all migrations.
After running it, check:

- tables;
- enums;
- functions;
- triggers;
- foreign keys;
- grants;
- RLS and policies.

## 3.5 Get the PostgreSQL connection

In Supabase:

```text
Connect
> Session pooler
> URI
```

The Session pooler is useful when the machine uses IPv4.

Do not confuse:

```text
API URL: https://PROJECT_REF.supabase.co
PostgreSQL connection: postgresql://...pooler.supabase.com:5432/postgres
```

## 3.6 Identify the dump format

Test first:

```powershell
pg_restore --list "C:\Migration\project.backup"
```

- If a table of contents appears, the file is a non-text PostgreSQL archive: use `pg_restore`.
- If the file is plain SQL text, use `psql -f`.

PostgreSQL requires `pg_restore` for custom or directory formats.

## 3.7 Restore a custom dump

Recommended method to avoid putting the password in the command:

```powershell
$env:PGPASSWORD="YOUR_DATABASE_PASSWORD"

pg_restore `
  --host="SESSION_POOLER_HOST" `
  --port="5432" `
  --username="postgres.PROJECT_REF" `
  --dbname="postgres" `
  --no-owner `
  --no-privileges `
  --verbose `
  "C:\Migration\project.backup"

Remove-Item Env:PGPASSWORD
```

Do not use `--clean` by reflex on a hosted Supabase. A global clean can affect
platform-managed objects.

## 3.8 Understand the warnings

A full dump may contain internal objects:

- event triggers;
- roles;
- extensions;
- functions owned by `supabase_admin`;
- items from the `auth`, `storage`, `extensions` or `realtime` schemas.

The PostgreSQL role available to the client is not a full superuser. Errors on those
objects can be expected.

The right question is not "were there zero errors?" but:

> Are the business objects, data, Auth, profiles, application functions and RLS
> consistent?

## 3.9 Verify tables and rows

```sql
SELECT
  schemaname,
  relname AS table_name,
  n_live_tup AS estimated_rows
FROM pg_stat_user_tables
ORDER BY schemaname, relname;
```

For an exact check:

```sql
SELECT COUNT(*) FROM public.table_name;
```

A very recent project may naturally have almost all its tables at zero rows.

## 3.10 Verify Auth

```sql
SELECT
  u.id,
  u.email,
  u.email_confirmed_at,
  COALESCE(u.encrypted_password <> '', false) AS password_hash_present,
  EXISTS (
    SELECT 1
    FROM auth.identities i
    WHERE i.user_id = u.id
  ) AS identity_present,
  (
    SELECT string_agg(i.provider, ', ')
    FROM auth.identities i
    WHERE i.user_id = u.id
  ) AS providers
FROM auth.users u;
```

Expected state for an email/password account:

```text
password_hash_present = true
identity_present = true
providers = email
email_confirmed_at = a date
```

### Incomplete account

If `auth.users` exists but `auth.identities` is missing:

- do not create an identity manually in SQL;
- for a new project with no business data, recreate the account from the Supabase Dashboard with automatic confirmation;
- for a real production, prepare a dedicated Auth migration and avoid any deletion without dependency analysis.

## 3.11 Verify the profile

The `profiles` table may have:

- `id`: the profile's own identifier;
- `user_id`: a reference to `auth.users.id`.

Correct join:

```sql
SELECT
  u.id AS auth_user_id,
  u.email,
  p.*
FROM auth.users u
LEFT JOIN public.profiles p
  ON p.user_id = u.id;
```

Check the foreign key:

```sql
SELECT
  conname,
  pg_get_constraintdef(oid)
FROM pg_constraint
WHERE conrelid = 'public.profiles'::regclass
  AND contype = 'f';
```

Check the creation trigger:

```sql
SELECT
  t.tgname AS trigger_name,
  pn.nspname AS function_schema,
  p.proname AS function_name
FROM pg_trigger t
JOIN pg_class c ON c.oid = t.tgrelid
JOIN pg_namespace cn ON cn.oid = c.relnamespace
JOIN pg_proc p ON p.oid = t.tgfoid
JOIN pg_namespace pn ON pn.oid = p.pronamespace
WHERE cn.nspname = 'auth'
  AND c.relname = 'users'
  AND NOT t.tgisinternal;
```

## 3.12 Verify RLS

Enablement:

```sql
SELECT
  c.relname AS table_name,
  c.relrowsecurity AS rls_enabled,
  COUNT(p.policyname) AS policy_count
FROM pg_class c
JOIN pg_namespace n ON n.oid = c.relnamespace
LEFT JOIN pg_policies p
  ON p.schemaname = n.nspname
  AND p.tablename = c.relname
WHERE n.nspname = 'public'
  AND c.relkind = 'r'
GROUP BY c.relname, c.relrowsecurity
ORDER BY c.relname;
```

Policy content:

```sql
SELECT
  tablename,
  policyname,
  roles,
  cmd,
  qual AS using_condition,
  with_check AS insert_update_condition
FROM pg_policies
WHERE schemaname = 'public'
ORDER BY tablename, policyname;
```

Typical condition:

```sql
USING (auth.uid() = user_id)
WITH CHECK (auth.uid() = user_id)
```

A policy being present is not enough: its condition must be audited.

## 3.13 Configure Auth URLs

In Supabase:

```text
Authentication
> URL Configuration
```

Example:

```text
Site URL
https://your-app.example

Redirect URLs
https://your-app.example/**
http://localhost:5173/**
```

Add the callback, reset-password and preview routes you actually use.

---

<!-- nav-footer -->

<div align="center">

[← 2. Preventing Lovable Cloud lock-in](02-prevent-cloud.md) &nbsp;·&nbsp; [English home](../en.md) &nbsp;·&nbsp; [4. Switchover & validation →](04-switch-validation.md)

</div>
