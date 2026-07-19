---
layout: default
title: Prévenir Lovable Cloud
---

# 2. Prévenir l’enfermement dans Lovable Cloud

## Pourquoi intervenir avant la première fonctionnalité backend

Lovable Cloud peut fournir automatiquement base, Auth, Storage et fonctions. Selon la configuration du workspace, l’agent peut activer Cloud lorsqu’une demande nécessite un backend.

La documentation Lovable indique que le comportement peut être réglé dans les permissions du connecteur Lovable Cloud avec trois choix :

- **Always allow** : activation sans validation ;
- **Ask each time** : validation humaine à chaque besoin ;
- **Never allow** : blocage de l’activation Cloud.

Pour un projet qui doit rester portable, choisissez **Never allow** ou, au minimum, **Ask each time** avant de demander une fonctionnalité backend.

## Étape A — Régler les permissions Lovable Cloud

Le libellé exact peut évoluer. Le chemin documenté est proche de :

```text
Workspace / Settings
> Connectors
> App connectors
> Lovable Cloud
> Manage permissions
```

Réglez **Enable Cloud** sur :

```text
Never allow
```

ou :

```text
Ask each time
```

Cette préférence évite l’activation automatique. Elle ne migre pas un projet déjà rattaché à Lovable Cloud.

## Étape B — Créer le Supabase externe

Dans Supabase :

1. créer une organisation ou utiliser une organisation existante ;
2. créer un projet dans la région adaptée ;
3. conserver le mot de passe PostgreSQL dans un gestionnaire de mots de passe ;
4. noter le Project ID et la Project URL ;
5. ne jamais publier la `service_role` ou la secret key.

## Étape C — Connecter Supabase à Lovable

Le chemin officiel est généralement :

```text
Project Settings
> Integrations
> Supabase
> Connect Supabase
```

Selon la version de l’interface, la connexion peut aussi être accessible depuis le menu des connecteurs.

Après autorisation :

1. choisir l’organisation Supabase ;
2. sélectionner le projet existant ;
3. vérifier que le Project ID correspond au projet attendu ;
4. ne pas créer un nouveau projet si le projet cible existe déjà.

## Étape D — Connecter GitHub

Dans Lovable :

```text
Project Settings
> Git
> Connect GitHub
```

Vérifiez :

- le bon compte ou la bonne organisation ;
- la branche `main` ;
- le statut `Connected` ;
- la création du dépôt attendu ;
- l’absence de secrets dans le dépôt.

Une fois connecté, le dépôt devient la référence du code et permet de travailler avec d’autres outils.

## Étape E — Configurer le backend avant de le générer

Avant de demander « ajoute l’authentification » ou « crée la base », préparez :

1. les fournisseurs Auth nécessaires ;
2. le Site URL et les Redirect URLs ;
3. la convention `user_id` ;
4. les politiques RLS attendues ;
5. le dossier `supabase/migrations/` ;
6. les secrets côté serveur ;
7. les tests d’isolation multi-utilisateur.

## Prompt de garde-fou

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

## Contrôle de sortie

Le projet neuf est prêt lorsque :

- Lovable Cloud ne peut pas s’activer automatiquement ;
- GitHub est connecté ;
- Supabase est connecté ;
- le Project ID est vérifié ;
- les variables frontend sont publiques ;
- les secrets sont stockés côté serveur ;
- les migrations seront versionnées.

---

<!-- nav-footer -->

<div align="center">

[← 1. Méthode Gab-E KISS](01-methode-gab-e-kiss.md) &nbsp;·&nbsp; [🏠 Accueil](index.md) &nbsp;·&nbsp; [3. Migration de rattrapage →](03-migration-rattrapage.md)

</div>
