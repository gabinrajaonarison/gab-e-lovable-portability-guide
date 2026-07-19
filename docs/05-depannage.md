---
layout: default
title: Dépannage
---

# 5. Dépannage

## `psql` indique que le dump est au format custom

**Cause** : le fichier est une archive PostgreSQL non texte.

**Action** : utiliser `pg_restore`, pas `psql -f`.

```powershell
pg_restore --list "C:\Migration\project.backup"
```

## `pg_restore` termine avec de nombreuses erreurs

Analysez d’abord les premières erreurs significatives.

Les event triggers, extensions et objets appartenant à `supabase_admin` peuvent échouer faute de privilèges superuser. Vérifiez ensuite séparément :

- tables métier ;
- données ;
- Auth ;
- profils ;
- triggers applicatifs ;
- RLS.

Ne lancez pas `--clean` sans comprendre précisément ce qui sera supprimé.

## Les tables existent mais affichent zéro ligne

- vérifier si l’ancien projet avait réellement des données ;
- utiliser des `COUNT(*)` exacts ;
- inspecter l’archive avec `pg_restore --list` ;
- rechercher `TABLE DATA public` ;
- restaurer en `--data-only --schema=public` uniquement si le schéma existe déjà et si l’archive contient les données attendues.

## L’utilisateur existe mais `identity_present = false`

- ne pas insérer directement dans `auth.identities` ;
- vérifier `email_confirmed_at` ;
- vérifier le profil et les dépendances ;
- pour un projet neuf, recréer le compte via le Dashboard Supabase ;
- pour une production, préparer une stratégie de migration Auth sans suppression aveugle.

## Le profil semble absent

Vérifier la colonne de jointure :

```text
profiles.id      = identifiant du profil
profiles.user_id = référence vers auth.users.id
```

La jointure se fait souvent avec `profiles.user_id`.

## La suppression de l’utilisateur supprime le profil

Vérifier la contrainte :

```sql
FOREIGN KEY (user_id) REFERENCES auth.users(id) ON DELETE CASCADE
```

Sauvegarder ou documenter les valeurs du profil avant une suppression.

## Le trigger de profil bloque la création d’utilisateur

Vérifier la fonction `handle_new_user` et le trigger `on_auth_user_created`. Une erreur de colonne, de type ou de contrainte peut provoquer une erreur lors du signup.

## Auth redirige vers une mauvaise page

Vérifier :

- Site URL ;
- Redirect URLs ;
- route de callback ;
- URL de preview ;
- URL publiée ;
- domaine personnalisé ;
- protocole `https` ;
- jokers autorisés.

## GitHub affiche `installation access required`

- vérifier le compte GitHub actif ;
- vérifier la GitHub App Lovable dans `Settings > Applications` ;
- autoriser les popups ;
- actualiser l’installation ;
- révoquer puis réinstaller l’autorisation si nécessaire ;
- choisir le compte propriétaire ;
- ne pas renommer ou déplacer le dépôt après connexion.

## Le frontend pointe encore vers l’ancien backend

Rechercher dans le dépôt :

```text
VITE_SUPABASE_URL
VITE_SUPABASE_PROJECT_ID
createClient(
*.supabase.co
```

Mettre à jour les variables, reconstruire et republier.

## RLS est activé mais les utilisateurs voient les données des autres

L’activation seule ne suffit pas. Inspecter :

```sql
SELECT * FROM pg_policies WHERE schemaname = 'public';
```

Une condition `true` ou une policy trop large peut exposer toutes les lignes. Tester avec deux comptes réels.

## Le dépôt contient un `.env`

Si le fichier contient un secret :

1. révoquer et régénérer immédiatement le secret ;
2. retirer le fichier du suivi Git ;
3. nettoyer l’historique si le secret a été poussé ;
4. ajouter `.env` dans `.gitignore` ;
5. conserver seulement `.env.example`.

---

<!-- nav-footer -->

<div align="center">

[← 4. Bascule et validation](04-bascule-validation.md) &nbsp;·&nbsp; [🏠 Accueil](index.md) &nbsp;·&nbsp; [6. Checklists →](06-checklists.md)

</div>
