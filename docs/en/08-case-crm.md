---
layout: default
title: 8. Case study — a CRM project
parent: English
nav_order: 9
---

> 🌐 Version française : [8. Cas pratique : projet CRM](../08-cas-pratique-crm.md)

# 8. Case study: recovering a CRM project

## Context

A CRM project was launched on Lovable with Lovable Cloud. The project was recent and
held little data. The goal was to regain control of the backend with an external
Supabase, then save the code on GitHub.

Identifiers, emails, UUIDs and keys were deliberately removed from this public version.

## Sequence actually followed

```text
Lovable Cloud already active
        |
        v
Export the PostgreSQL dump
        |
        v
Create an external Supabase
        |
        v
Replay the schema and restore with pg_restore
        |
        v
Check tables and data
        |
        v
Check Auth / identities / profiles
        |
        v
Repair the incomplete Auth account
        |
        v
Check the handle_new_user trigger and FK cascade
        |
        v
Check RLS and auth.uid() = user_id policies
        |
        v
Configure Site URL and Redirect URLs
        |
        v
Connect GitHub and sync the repository
        |
        v
Prepare the switchover to the external Supabase
```

## Incident 1 — Wrong restore tool

The first command used `psql -f`, but PostgreSQL reported that the dump was in custom
format.

Fix:

```powershell
pg_restore --list "project.backup"
```

then restore with `pg_restore`.

## Incident 2 — Many permission errors

The restore finished with several errors on event triggers and internal functions.

Diagnosis: the dump contained platform-managed objects and the client role was not a
full superuser.

Decision: do not rerun with `--clean`. Verify the business objects separately.

Result: the application tables were present.

## Incident 3 — Partially restored Auth user

The user existed in `auth.users`, the password hash was present, but:

```text
identity_present = false
providers = NULL
email_confirmed_at = NULL
```

The public profile existed and was correctly linked via `profiles.user_id`.

Since the project was new and had no important business data, the account was recreated
cleanly from the Supabase Dashboard with automatic confirmation.

Final result:

```text
provider = email
email_confirmed_at = a date
profiles.user_id = auth.users.id
```

## Incident 4 — Confusion between `profiles.id` and `profiles.user_id`

A query looked for the Auth UUID in `profiles.id` and returned no rows.

The table used:

```text
profiles.id      = the profile identifier
profiles.user_id = the Auth identifier
```

The correct join confirmed the link.

## Incident 5 — GitHub installation access required

Lovable detected the GitHub installation but could not confirm the permissions.

The resolution was to:

- verify the active GitHub account;
- verify the Lovable GitHub App installation;
- refresh or reinstall the authorization;
- reconnect the repository from the right workspace.

The repository was then created and the Lovable status became `Connected`.

## Result

At the end of the process:

- code synced on GitHub;
- tables restored;
- Auth working;
- profile linked;
- profile trigger present;
- RLS active;
- policies by `user_id` validated;
- Auth URLs configured;
- external Supabase ready for the switchover.

## Main lesson

Catch-up is possible, but it is far simpler to apply the **Ownership Gate** before the
first backend request.

---

<!-- nav-footer -->

<div align="center">

[← 7. Multi-LLM portability](07-multi-llm.md) &nbsp;·&nbsp; [English home](../en.md) &nbsp;·&nbsp; [9. Official sources →](09-sources.md)

</div>
