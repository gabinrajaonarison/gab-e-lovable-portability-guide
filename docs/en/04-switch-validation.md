---
layout: default
title: 4. Switchover and validation
parent: English
nav_order: 5
---

> 🌐 Version française : [4. Bascule et validation](../04-bascule-validation.md)

# 4. Switchover, security and validation

## 4.1 What the dump alone does not migrate

Before the cutover, inspect separately:

- **Storage**: download the physical files;
- **Secrets**: note the names and recreate the values on the target platform;
- **Jobs**: document frequency, payload and dependencies;
- **Edge Functions**: back up the code and redeploy;
- **Auth providers**: reconfigure Email, Google or others;
- **Auth emails**: templates, SMTP and URLs;
- **External services**: Stripe, business APIs, webhooks.

A Lovable internal secret must not be copied into the frontend.

## 4.2 Sync GitHub

The repository must contain at least:

```text
src/
public/
supabase/
package.json
README.md
.env.example
```

The repository must not contain:

```text
real .env
service_role
secret key
PostgreSQL password
JWT secret
database backup
```

## 4.3 Recommended switchover order

1. Freeze business changes.
2. Make a final Lovable Cloud export.
3. Verify the local backups.
4. Verify GitHub is `Connected`.
5. Verify tables, Auth, profiles, triggers, RLS and Storage in the target Supabase.
6. Remove Lovable Cloud only after validation.
7. Connect the external Supabase in the project integrations.
8. Update the variables.
9. Republish the app.
10. Run the smoke tests.

## 4.4 Remove Lovable Cloud

The Lovable documentation gives the path:

```text
Cloud
> Overview
> Advanced settings
> Remove Lovable Cloud
```

This operation permanently deletes the database, Auth, Storage and functions managed
by Lovable Cloud.

Before confirming:

- database export done;
- Storage files downloaded;
- GitHub synchronized;
- external Supabase restored;
- Auth and RLS validated;
- secret names listed;
- rollback documented.

## 4.5 Connect the external Supabase

In the Lovable project:

```text
Project Settings
> Integrations
> Supabase
> Connect Supabase
```

Select the organization then the exact project. Verify the Project ID before
confirming.

## 4.6 Frontend variables

```env
VITE_SUPABASE_URL=https://PROJECT_REF.supabase.co
VITE_SUPABASE_PROJECT_ID=PROJECT_REF
VITE_SUPABASE_PUBLISHABLE_KEY=sb_publishable_xxx
```

Variables forbidden in the frontend:

```text
SUPABASE_SERVICE_ROLE_KEY
SUPABASE_SECRET_KEY
DATABASE_PASSWORD
POSTGRES_PASSWORD
JWT_SECRET
```

## 4.7 Local Supabase configuration

```toml
# supabase/config.toml
project_id = "PROJECT_REF"
```

Also search for old references:

```text
VITE_SUPABASE_URL
VITE_SUPABASE_PROJECT_ID
createClient(
*.supabase.co
```

## 4.8 Post-connection audit prompt

```text
CONTEXT
The project has just been connected to an external Supabase that is already restored and validated.

TASK
Audit all backend references and configure the frontend to use only this Supabase project.

GUIDELINES
Check the URL, the publishable key, the project ID, the Supabase client, config.toml and any old Cloud references.
Present the changes before applying.

CONSTRAINTS
Do not create any table.
Do not replay any migration.
Do not modify any RLS policy, function, trigger, enum or data.
Never use service_role in the frontend.
```

## 4.9 Smoke tests

In a private tab:

1. sign in;
2. read the profile;
3. create a company;
4. create a contact;
5. create a lead;
6. add to a list;
7. create a follow-up;
8. sign out then sign back in;
9. check the rows in Supabase;
10. check the network requests point to the new `PROJECT_REF.supabase.co`.

## 4.10 Multi-user test

Create a second account:

- account A only sees its own rows;
- account B only sees its own rows;
- inserts with another `user_id` are refused;
- server endpoints use an explicit authorization check.

This test truly validates the RLS policies.

---

<!-- nav-footer -->

<div align="center">

[← 3. Catch-up migration](03-migration.md) &nbsp;·&nbsp; [English home](../en.md) &nbsp;·&nbsp; [5. Troubleshooting →](05-troubleshooting.md)

</div>
