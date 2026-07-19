---
layout: default
title: 5. Troubleshooting
parent: English
nav_order: 6
---

> 🌐 Version française : [5. Dépannage](../05-depannage.md)

# 5. Troubleshooting

## `psql` says the dump is in custom format

**Cause**: the file is a non-text PostgreSQL archive.

**Action**: use `pg_restore`, not `psql -f`.

```powershell
pg_restore --list "C:\Migration\project.backup"
```

## `pg_restore` finishes with many errors

Analyze the first significant errors first.

Event triggers, extensions and objects owned by `supabase_admin` may fail for lack of
superuser privileges. Then check separately:

- business tables;
- data;
- Auth;
- profiles;
- application triggers;
- RLS.

Do not run `--clean` without precisely understanding what will be deleted.

## Tables exist but show zero rows

- check whether the old project actually had data;
- use exact `COUNT(*)`;
- inspect the archive with `pg_restore --list`;
- look for `TABLE DATA public`;
- restore with `--data-only --schema=public` only if the schema already exists and the archive contains the expected data.

## The user exists but `identity_present = false`

- do not insert directly into `auth.identities`;
- check `email_confirmed_at`;
- check the profile and its dependencies;
- for a new project, recreate the account via the Supabase Dashboard;
- for production, prepare an Auth migration strategy with no blind deletion.

## The profile seems missing

Check the join column:

```text
profiles.id      = the profile identifier
profiles.user_id = reference to auth.users.id
```

The join is often done on `profiles.user_id`.

## Deleting the user deletes the profile

Check the constraint:

```sql
FOREIGN KEY (user_id) REFERENCES auth.users(id) ON DELETE CASCADE
```

Back up or document the profile values before a deletion.

## The profile trigger blocks user creation

Check the `handle_new_user` function and the `on_auth_user_created` trigger. A column,
type or constraint error can cause an error during signup.

## Auth redirects to the wrong page

Check:

- Site URL;
- Redirect URLs;
- callback route;
- preview URL;
- published URL;
- custom domain;
- `https` protocol;
- allowed wildcards.

## GitHub shows `installation access required`

- check the active GitHub account;
- check the Lovable GitHub App under `Settings > Applications`;
- allow popups;
- refresh the installation;
- revoke then reinstall the authorization if needed;
- choose the owner account;
- do not rename or move the repository after connecting.

## The frontend still points to the old backend

Search the repository for:

```text
VITE_SUPABASE_URL
VITE_SUPABASE_PROJECT_ID
createClient(
*.supabase.co
```

Update the variables, rebuild and republish.

## RLS is enabled but users see each other's data

Enabling it is not enough. Inspect:

```sql
SELECT * FROM pg_policies WHERE schemaname = 'public';
```

A `true` condition or an overly broad policy can expose every row. Test with two real
accounts.

## The repository contains a `.env`

If the file contains a secret:

1. revoke and regenerate the secret immediately;
2. remove the file from Git tracking;
3. clean the history if the secret was pushed;
4. add `.env` to `.gitignore`;
5. keep only `.env.example`.

---

<!-- nav-footer -->

<div align="center">

[← 4. Switchover & validation](04-switch-validation.md) &nbsp;·&nbsp; [English home](../en.md) &nbsp;·&nbsp; [6. Operational checklists →](06-checklists.md)

</div>
