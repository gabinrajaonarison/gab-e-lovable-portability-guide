---
layout: default
title: Migration de rattrapage
nav_order: 4
---

# 3. Migration de rattrapage : Lovable Cloud vers votre Supabase

Ce parcours s’applique lorsqu’un projet utilise déjà Lovable Cloud et doit être repris dans un projet Supabase contrôlé par son propriétaire.

## 3.1 Geler et inventorier

Avant toute action irréversible :

- arrêter temporairement les modifications fonctionnelles ;
- noter les tables, enums, fonctions, triggers et politiques ;
- recenser Auth, providers et utilisateurs ;
- recenser les buckets et fichiers Storage ;
- recenser Edge Functions, Jobs et Secrets ;
- noter l’URL publique et les URLs Auth ;
- connecter GitHub si ce n’est pas déjà fait.

## 3.2 Exporter Lovable Cloud

La documentation Lovable permet désormais d’exporter la base depuis :

```text
Cloud
> Overview
> Advanced settings
> Export project data
```

L’export de la base et le téléchargement des fichiers Storage sont deux opérations distinctes.

Conservez le dump :

- sur votre ordinateur ;
- dans une sauvegarde chiffrée ;
- hors du dépôt GitHub.

## 3.3 Créer le Supabase cible

Créez un projet vide dans votre propre organisation et relevez :

```text
Project ID
Project URL
Database password
Region
Publishable key
```

Ne partagez jamais le mot de passe de base, la `service_role` ou la secret key.

## 3.4 Rejouer le schéma

La méthode la plus traçable consiste à exécuter les migrations de :

```text
supabase/migrations/
```

Dans l’ordre chronologique.

Un script SQL consolidé peut être utilisé lorsqu’il représente fidèlement toutes les migrations. Après exécution, contrôlez :

- tables ;
- enums ;
- fonctions ;
- triggers ;
- clés étrangères ;
- grants ;
- RLS et policies.

## 3.5 Récupérer la connexion PostgreSQL

Dans Supabase :

```text
Connect
> Session pooler
> URI
```

Le Session pooler est utile lorsque le poste utilise IPv4.

Ne confondez pas :

```text
URL API : https://PROJECT_REF.supabase.co
Connexion PostgreSQL : postgresql://...pooler.supabase.com:5432/postgres
```

## 3.6 Identifier le format du dump

Testez d’abord :

```powershell
pg_restore --list "C:\Migration\project.backup"
```

- Si une table des matières apparaît, le fichier est une archive PostgreSQL non texte : utilisez `pg_restore`.
- Si le fichier est du SQL texte, utilisez `psql -f`.

PostgreSQL impose `pg_restore` pour les formats custom ou directory.

## 3.7 Restaurer un dump custom

Méthode recommandée pour éviter de placer le mot de passe dans la commande :

```powershell
$env:PGPASSWORD="VOTRE_MOT_DE_PASSE_DATABASE"

pg_restore `
  --host="HOTE_SESSION_POOLER" `
  --port="5432" `
  --username="postgres.PROJECT_REF" `
  --dbname="postgres" `
  --no-owner `
  --no-privileges `
  --verbose `
  "C:\Migration\project.backup"

Remove-Item Env:PGPASSWORD
```

N’utilisez pas `--clean` par réflexe sur un Supabase hébergé. Un nettoyage global peut toucher des objets gérés par la plateforme.

## 3.8 Comprendre les avertissements

Un dump complet peut contenir des objets internes :

- event triggers ;
- rôles ;
- extensions ;
- fonctions appartenant à `supabase_admin` ;
- éléments des schémas `auth`, `storage`, `extensions` ou `realtime`.

Le rôle PostgreSQL accessible au client n’est pas un superutilisateur complet. Des erreurs sur ces objets peuvent être attendues.

La bonne question n’est pas « y a-t-il eu zéro erreur ? », mais :

> Les objets métier, les données, Auth, profils, fonctions applicatives et RLS sont-ils cohérents ?

## 3.9 Vérifier les tables et les lignes

```sql
SELECT
  schemaname,
  relname AS table_name,
  n_live_tup AS estimated_rows
FROM pg_stat_user_tables
ORDER BY schemaname, relname;
```

Pour un contrôle exact :

```sql
SELECT COUNT(*) FROM public.nom_de_table;
```

Un projet très récent peut naturellement avoir presque toutes ses tables à zéro ligne.

## 3.10 Vérifier Auth

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

État attendu pour un compte email/mot de passe :

```text
password_hash_present = true
identity_present = true
providers = email
email_confirmed_at = une date
```

### Compte incomplet

Si `auth.users` existe mais `auth.identities` est absent :

- ne créez pas une identité manuellement en SQL ;
- pour un projet neuf sans données métier, recréez le compte depuis le Dashboard Supabase avec confirmation automatique ;
- pour une production réelle, préparez une migration Auth dédiée et évitez toute suppression sans analyse des dépendances.

## 3.11 Vérifier le profil

La table `profiles` peut avoir :

- `id` : identifiant propre du profil ;
- `user_id` : référence vers `auth.users.id`.

Jointure correcte :

```sql
SELECT
  u.id AS auth_user_id,
  u.email,
  p.*
FROM auth.users u
LEFT JOIN public.profiles p
  ON p.user_id = u.id;
```

Vérifiez la clé étrangère :

```sql
SELECT
  conname,
  pg_get_constraintdef(oid)
FROM pg_constraint
WHERE conrelid = 'public.profiles'::regclass
  AND contype = 'f';
```

Vérifiez le trigger de création :

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

## 3.12 Vérifier RLS

Activation :

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

Contenu des policies :

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

Condition classique :

```sql
USING (auth.uid() = user_id)
WITH CHECK (auth.uid() = user_id)
```

La présence d’une policy ne suffit pas : sa condition doit être auditée.

## 3.13 Configurer Auth URLs

Dans Supabase :

```text
Authentication
> URL Configuration
```

Exemple :

```text
Site URL
https://votre-app.example

Redirect URLs
https://votre-app.example/**
http://localhost:5173/**
```

Ajoutez les routes de callback, reset password et preview réellement utilisées.

---

<!-- nav-footer -->

<div align="center">

[← 2. Prévenir Lovable Cloud](02-prevenir-lovable-cloud.md) &nbsp;·&nbsp; [🏠 Accueil](index.md) &nbsp;·&nbsp; [4. Bascule et validation →](04-bascule-validation.md)

</div>
