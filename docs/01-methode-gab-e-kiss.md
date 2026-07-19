---
layout: default
title: Méthode Gab-E KISS
nav_order: 2
---

# 1. The Gab-E Méthode (KISS)

## La méthode d’origine

La méthode Gab-E KISS vise à passer rapidement d’une idée à un premier produit testable :

1. **Start with an Idea** — environ 5 minutes.
2. **Find Design Inspiration** — environ 5 minutes sur Dribbble ou une source équivalente : formes, couleurs, densité, composants.
3. **Design the User Flow** — environ 30 minutes sur Whimsical, Miro ou un outil similaire, en restant centré sur la proposition de valeur principale.
4. **Build the Master Prompt** — manuellement ou avec Gab-E Master Prompt.
5. **Give the Master Prompt to Lovable** — joindre le user flow, les références visuelles et le thème.
6. **Let Lovable build it out.**
7. **Test, iterate, repeat.** Ne pas avoir peur de casser dans un environnement contrôlé.

## Le garde-fou ajouté : Ownership First

Le problème n’était pas la vitesse de Lovable, mais l’absence d’une étape explicite de propriété technique.

La méthode renforcée ajoute donc un **Gate de portabilité** avant toute demande backend :

```text
Avant Auth / Database / Storage :

1. Choisir la politique Lovable Cloud.
2. Connecter GitHub.
3. Créer et connecter le Supabase externe.
4. Préparer les variables et les migrations.
5. Autoriser ensuite Lovable à construire le backend.
```

## Méthode renforcée en huit étapes

1. **Idée** : problème, cible, résultat attendu.
2. **Inspiration** : style, composants, hiérarchie visuelle.
3. **User flow** : parcours principal, exceptions et états vides.
4. **Master Prompt** : contexte, tâche, guidelines et contraintes.
5. **Ownership Gate** : GitHub, Supabase, permissions Cloud, secrets.
6. **Construction Lovable** : frontend et backend reliés au socle contrôlé.
7. **Validation** : Auth, CRUD, RLS, erreurs, responsive, performance.
8. **Itération multi-LLM** : Lovable, Claude, ChatGPT, Cursor ou IDE local à partir du même dépôt.

## Les actifs à contrôler

| Actif | Source de vérité recommandée |
|---|---|
| Code | GitHub |
| Schéma de base | `supabase/migrations/` |
| Données | Supabase + sauvegardes contrôlées |
| Auth | Supabase externe |
| Fichiers | Supabase Storage ou stockage externe |
| Secrets | Gestionnaire de secrets de la plateforme serveur |
| Documentation | GitHub Pages |
| Prompts structurants | dépôt GitHub ou base documentaire versionnée |

## Principes non négociables

- GitHub est connecté tôt.
- Le Supabase externe est connecté avant la première fonctionnalité backend.
- Les migrations SQL sont versionnées.
- Le frontend ne reçoit que des clés publiques.
- Les secrets serveur ne commencent jamais par `VITE_`.
- RLS est activé et testé avec au moins deux comptes.
- Le fichier `.env.example` ne contient aucune vraie valeur sensible.
- La procédure de sortie est documentée avant d’en avoir besoin.
- Une suppression de Lovable Cloud n’est effectuée qu’après export, restauration et tests.

## Structure recommandée du Master Prompt

```text
CONTEXT
Localiser le projet, la page, le parcours et l’infrastructure cible.

TASK
Décrire précisément le résultat à produire.

GUIDELINES
Préciser la stack, les conventions, les migrations, Auth, RLS et tests.

CONSTRAINTS
Interdire l’activation de Lovable Cloud, l’exposition de secrets, les modifications hors périmètre et les migrations destructrices non validées.
```

---

<!-- nav-footer -->

<div align="center">

[← Accueil](index.md) &nbsp;·&nbsp; [🏠 Accueil](index.md) &nbsp;·&nbsp; [2. Prévenir Lovable Cloud →](02-prevenir-lovable-cloud.md)

</div>
