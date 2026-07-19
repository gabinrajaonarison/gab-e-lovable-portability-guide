---
layout: default
title: Accueil
---

# The Gab-E Méthode (KISS)

## Construire vite avec Lovable, sans perdre la maîtrise du code ni du backend

Lovable est un excellent accélérateur pour transformer une idée en application. Le risque apparaît lorsque le code, la base, l’authentification, le stockage et les secrets deviennent difficiles à déplacer vers un autre environnement.

Ce guide formalise une règle simple :

> **Lovable doit rester un accélérateur de construction, pas l’unique point de contrôle du projet.**

Le socle cible est le suivant :

```text
Idée + User flow + Master Prompt
              |
              v
        Lovable AI Agent
              |
              | génère et synchronise le code
              v
GitHub = source de vérité du code
              |
              +------> Claude / ChatGPT / Codex / Cursor / IDE local
              |
              v
Supabase externe = PostgreSQL + Auth + Storage + Functions
              |
              v
Lovable Publish / Vercel / Netlify / autre hébergement
```

## Deux parcours

### Parcours A — Préventif

À utiliser pour un nouveau projet :

1. définir les permissions Lovable Cloud ;
2. connecter GitHub ;
3. créer et connecter son Supabase ;
4. seulement ensuite demander Auth, base ou stockage à Lovable.

### Parcours B — Rattrapage

À utiliser lorsqu’un projet est déjà sur Lovable Cloud :

1. inventorier et exporter ;
2. restaurer dans un Supabase contrôlé ;
3. réparer les éléments incomplets ;
4. vérifier Auth, profils, triggers et RLS ;
5. connecter GitHub ;
6. basculer l’application ;
7. supprimer Lovable Cloud uniquement après validation.

## Navigation

- [1. Méthode Gab-E KISS](01-methode-gab-e-kiss.md)
- [2. Prévenir l’enfermement dans Lovable Cloud](02-prevenir-lovable-cloud.md)
- [3. Migration de rattrapage Lovable Cloud vers Supabase](03-migration-rattrapage.md)
- [4. Bascule, sécurité et validation](04-bascule-validation.md)
- [5. Dépannage](05-depannage.md)
- [6. Checklists opérationnelles](06-checklists.md)
- [7. Portabilité vers Claude, ChatGPT et autres LLM](07-portabilite-multi-llm.md)
- [8. Cas pratique ProspectFlow CRM](08-cas-pratique-prospectflow.md)
- [9. Sources officielles](09-sources-officielles.md)

## Avertissement

Les suppressions de backend, les restaurations PostgreSQL et les changements d’Auth peuvent être irréversibles. Travaillez d’abord sur un projet neuf ou un environnement de test, conservez plusieurs sauvegardes et ne copiez jamais de secrets dans un prompt ou un dépôt public.

---

<!-- nav-footer -->

<div align="center">

**[Commencer le guide → 1. Méthode Gab-E KISS](01-methode-gab-e-kiss.md)**

</div>
