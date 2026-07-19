---
layout: default
title: Bascule et validation
nav_order: 5
---

# 4. Bascule, sécurité et validation

## 4.1 Ce que le dump ne suffit pas à migrer

Avant la coupure, inspectez séparément :

- **Storage** : télécharger les fichiers physiques ;
- **Secrets** : relever les noms et recréer les valeurs dans la plateforme cible ;
- **Jobs** : documenter fréquence, payload et dépendances ;
- **Edge Functions** : sauvegarder le code et redéployer ;
- **Providers Auth** : reconfigurer Email, Google ou autres ;
- **Emails Auth** : templates, SMTP et URLs ;
- **Services externes** : Stripe, APIs métier, webhooks.

Un secret interne Lovable ne doit pas être copié dans le frontend.

## 4.2 Synchroniser GitHub

Le dépôt doit contenir au minimum :

```text
src/
public/
supabase/
package.json
README.md
.env.example
```

Le dépôt ne doit pas contenir :

```text
.env réel
service_role
secret key
mot de passe PostgreSQL
JWT secret
backup de base
```

## 4.3 Ordre de bascule recommandé

1. Geler les modifications métier.
2. Faire un dernier export Lovable Cloud.
3. Vérifier les sauvegardes locales.
4. Vérifier que GitHub est `Connected`.
5. Vérifier tables, Auth, profils, triggers, RLS et Storage dans le Supabase cible.
6. Retirer Lovable Cloud seulement après validation.
7. Connecter le Supabase externe dans les intégrations du projet.
8. Mettre à jour les variables.
9. Republier l’application.
10. Exécuter les tests de fumée.

## 4.4 Retirer Lovable Cloud

La documentation Lovable indique le chemin :

```text
Cloud
> Overview
> Advanced settings
> Remove Lovable Cloud
```

Cette opération supprime définitivement la base, Auth, Storage et les fonctions gérées par Lovable Cloud.

Avant de confirmer :

- export base terminé ;
- fichiers Storage téléchargés ;
- GitHub synchronisé ;
- Supabase externe restauré ;
- Auth et RLS validés ;
- noms de secrets recensés ;
- rollback documenté.

## 4.5 Connecter le Supabase externe

Dans le projet Lovable :

```text
Project Settings
> Integrations
> Supabase
> Connect Supabase
```

Sélectionnez l’organisation puis le projet exact. Vérifiez le Project ID avant confirmation.

## 4.6 Variables frontend

```env
VITE_SUPABASE_URL=https://PROJECT_REF.supabase.co
VITE_SUPABASE_PROJECT_ID=PROJECT_REF
VITE_SUPABASE_PUBLISHABLE_KEY=sb_publishable_xxx
```

Variables interdites dans le frontend :

```text
SUPABASE_SERVICE_ROLE_KEY
SUPABASE_SECRET_KEY
DATABASE_PASSWORD
POSTGRES_PASSWORD
JWT_SECRET
```

## 4.7 Configuration Supabase locale

```toml
# supabase/config.toml
project_id = "PROJECT_REF"
```

Recherchez également les anciennes références :

```text
VITE_SUPABASE_URL
VITE_SUPABASE_PROJECT_ID
createClient(
*.supabase.co
```

## 4.8 Prompt d’audit post-connexion

```text
CONTEXT
Le projet vient d’être relié à un Supabase externe déjà restauré et validé.

TASK
Audite toutes les références backend et configure le frontend pour utiliser uniquement ce projet Supabase.

GUIDELINES
Vérifie l’URL, la publishable key, le project ID, le client Supabase, config.toml et les anciennes références Cloud.
Présente les modifications avant application.

CONSTRAINTS
Ne crée aucune table.
Ne rejoue aucune migration.
Ne modifie aucune policy RLS, fonction, trigger, enum ou donnée.
N’utilise jamais service_role dans le frontend.
```

## 4.9 Tests de fumée

Dans un onglet privé :

1. connexion ;
2. lecture du profil ;
3. création d’une entreprise ;
4. création d’un contact ;
5. création d’un prospect ;
6. ajout à une liste ;
7. création d’une relance ;
8. déconnexion puis reconnexion ;
9. vérification des lignes dans Supabase ;
10. vérification des requêtes réseau vers le nouveau `PROJECT_REF.supabase.co`.

## 4.10 Test multi-utilisateur

Créez un second compte :

- le compte A ne voit que ses lignes ;
- le compte B ne voit que ses lignes ;
- les insertions avec un autre `user_id` sont refusées ;
- les endpoints serveur utilisent un contrôle d’autorisation explicite.

Ce test valide réellement les policies RLS.

---

<!-- nav-footer -->

<div align="center">

[← 3. Migration de rattrapage](03-migration-rattrapage.md) &nbsp;·&nbsp; [🏠 Accueil](index.md) &nbsp;·&nbsp; [5. Dépannage →](05-depannage.md)

</div>
