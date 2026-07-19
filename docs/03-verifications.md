[← Retour à l'accueil](index.md)

# Vérifications : Auth, `auth.identities`, profils, triggers, RLS

Après restauration, **ne faites confiance à rien tant que ce n'est pas vérifié.**
Voici les contrôles essentiels (à exécuter dans le SQL Editor de Supabase).

## 1. Comptes utilisateurs (`auth.users`)

```sql
select count(*) from auth.users;
```

Les utilisateurs vivent dans le schéma `auth`, géré par le service Auth de
Supabase. Selon la méthode d'export, il peut être nécessaire de recréer les
comptes ou d'importer les utilisateurs via l'API Admin plutôt que par un simple
dump SQL. Comparez le nombre attendu avec le nombre réel.

## 2. Identités (`auth.identities`)

```sql
select provider, count(*)
from auth.identities
group by provider;
```

Chaque compte email a normalement une identité `email`. Une incohérence ici =
connexions qui échoueront. Vérifiez que le nombre d'identités correspond au nombre
d'utilisateurs attendus par fournisseur.

## 3. Profils applicatifs

Beaucoup d'apps ont une table `public.profiles` liée à `auth.users` :

```sql
-- des utilisateurs sans profil ?
select u.id
from auth.users u
left join public.profiles p on p.id = u.id
where p.id is null;
```

Zéro ligne = chaque utilisateur a bien son profil.

## 4. Triggers

Le profil est souvent créé automatiquement par un **trigger** sur `auth.users`.
Vérifiez qu'il existe après migration :

```sql
select tgname, tgrelid::regclass as table
from pg_trigger
where not tgisinternal
order by table;
```

Si le trigger « à la création d'un utilisateur, insérer un profil » manque,
recréez-le (fonction + trigger) — sinon les **nouveaux** comptes n'auront pas de
profil.

## 5. RLS (Row Level Security)

C'est le point le plus sensible pour la sécurité.

```sql
-- Quelles tables publiques ont la RLS activée ?
select tablename, rowsecurity
from pg_tables
where schemaname = 'public'
order by tablename;
```

```sql
-- Tables avec RLS activée mais AUCUNE policy (accès totalement bloqué)
select t.tablename
from pg_tables t
left join pg_policies p
  on p.tablename = t.tablename and p.schemaname = t.schemaname
where t.schemaname = 'public' and t.rowsecurity = true
group by t.tablename
having count(p.policyname) = 0;
```

À retenir :

- Une table **sans RLS** = potentiellement lisible/modifiable par quiconque a la
  clé publique. **Danger.**
- Une table **avec RLS mais sans policy** = tout est bloqué (parfois voulu, souvent
  non).

Rejouez vos policies après migration, puis re-testez.

[← Migration](02-migration.md) · [Suivant : Bascule & tests →](04-bascule-et-tests.md)
