---
layout: default
title: Cas pratique ProspectFlow CRM
nav_order: 9
---

# 8. Cas pratique : récupération de ProspectFlow CRM

## Contexte

Le projet ProspectFlow CRM a été lancé sur Lovable avec Lovable Cloud. Le projet était récent et contenait peu de données. L’objectif était de reprendre la maîtrise du backend avec un Supabase externe, puis de sauvegarder le code sur GitHub.

Les identifiants, emails, UUID et clés ont été volontairement retirés de cette version publique.

## Séquence réellement suivie

```text
Lovable Cloud déjà actif
        |
        v
Export du dump PostgreSQL
        |
        v
Création d’un Supabase externe
        |
        v
Rejeu du schéma et restauration pg_restore
        |
        v
Contrôle des tables et des données
        |
        v
Contrôle Auth / identities / profiles
        |
        v
Réparation du compte Auth incomplet
        |
        v
Contrôle trigger handle_new_user et FK cascade
        |
        v
Contrôle RLS et policies auth.uid() = user_id
        |
        v
Configuration Site URL et Redirect URLs
        |
        v
Connexion GitHub et synchronisation du dépôt
        |
        v
Préparation de la bascule vers le Supabase externe
```

## Incident 1 — Mauvais outil de restauration

La première commande utilisait `psql -f`, mais PostgreSQL a indiqué que le dump était au format custom.

Correction :

```powershell
pg_restore --list "project.backup"
```

puis restauration avec `pg_restore`.

## Incident 2 — Nombreuses erreurs de permissions

La restauration a terminé avec plusieurs erreurs sur des event triggers et fonctions internes.

Diagnostic : le dump contenait des objets gérés par la plateforme et le rôle client n’était pas un superutilisateur complet.

Décision : ne pas relancer avec `--clean`. Vérifier les objets métier séparément.

Résultat : les tables applicatives étaient présentes.

## Incident 3 — Utilisateur Auth partiellement restauré

L’utilisateur existait dans `auth.users`, le hash du mot de passe était présent, mais :

```text
identity_present = false
providers = NULL
email_confirmed_at = NULL
```

Le profil public existait et était correctement lié via `profiles.user_id`.

Comme le projet était neuf et sans données métier importantes, le compte a été recréé proprement depuis le Dashboard Supabase avec confirmation automatique.

Résultat final :

```text
provider = email
email_confirmed_at = une date
profiles.user_id = auth.users.id
```

## Incident 4 — Confusion entre `profiles.id` et `profiles.user_id`

Une requête cherchait l’UUID Auth dans `profiles.id` et ne retournait aucune ligne.

La table utilisait :

```text
profiles.id      = identifiant du profil
profiles.user_id = identifiant Auth
```

La jointure correcte a confirmé le lien.

## Incident 5 — GitHub installation access required

Lovable détectait l’installation GitHub mais ne pouvait pas confirmer les permissions.

La résolution a consisté à :

- vérifier le compte GitHub actif ;
- vérifier l’installation de la GitHub App Lovable ;
- actualiser ou réinstaller l’autorisation ;
- reconnecter le dépôt depuis le bon workspace.

Le dépôt a ensuite été créé et le statut Lovable est devenu `Connected`.

## Résultat

À la fin du parcours :

- code synchronisé sur GitHub ;
- tables restaurées ;
- Auth fonctionnel ;
- profil lié ;
- trigger de profil présent ;
- RLS actif ;
- policies par `user_id` validées ;
- Auth URLs configurées ;
- Supabase externe prêt pour la bascule.

## Enseignement principal

Le rattrapage est possible, mais il est beaucoup plus simple d’appliquer le **Ownership Gate** avant la première demande backend.

---

<!-- nav-footer -->

<div align="center">

[← 7. Portabilité multi-LLM](07-portabilite-multi-llm.md) &nbsp;·&nbsp; [🏠 Accueil](index.md) &nbsp;·&nbsp; [9. Sources officielles →](09-sources-officielles.md)

</div>
