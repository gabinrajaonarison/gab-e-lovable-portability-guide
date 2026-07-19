---
layout: default
title: Checklists
---

# 6. Checklists opérationnelles

## Projet neuf

- [ ] Idée et périmètre MVP définis.
- [ ] Inspiration visuelle sélectionnée.
- [ ] User flow validé.
- [ ] Master Prompt structuré.
- [ ] `Enable Cloud` réglé sur `Never allow` ou `Ask each time`.
- [ ] Projet Supabase externe créé.
- [ ] Organisation Supabase autorisée dans Lovable.
- [ ] Projet Supabase relié au projet Lovable.
- [ ] GitHub connecté.
- [ ] Migrations versionnées.
- [ ] RLS activé et audité.
- [ ] Auth URLs configurées.
- [ ] Secrets uniquement côté serveur.
- [ ] Tests avec deux utilisateurs.

## Migration de rattrapage

- [ ] Export `.backup` téléchargé localement.
- [ ] Code synchronisé sur GitHub.
- [ ] Inventaire Storage / Secrets / Jobs / Edge Functions réalisé.
- [ ] Supabase cible créé.
- [ ] Schéma restauré.
- [ ] Format du dump identifié.
- [ ] Données restaurées avec l’outil approprié.
- [ ] Tables et nombres de lignes vérifiés.
- [ ] `auth.users` et `auth.identities` vérifiés.
- [ ] Profil et trigger de création vérifiés.
- [ ] RLS et policies vérifiés.
- [ ] Site URL et Redirect URLs configurés.
- [ ] Dernier export effectué avant coupure.

## Bascule

- [ ] Fenêtre de maintenance définie.
- [ ] Ancien backend gelé.
- [ ] Dernier export terminé.
- [ ] Sauvegardes testées.
- [ ] GitHub `Connected`.
- [ ] Lovable Cloud supprimé seulement après validation.
- [ ] Supabase externe connecté.
- [ ] Variables du nouveau Supabase appliquées.
- [ ] `supabase/config.toml` mis à jour.
- [ ] Application republiée.
- [ ] Connexion Auth testée.
- [ ] CRUD métier testé.
- [ ] Isolation multi-utilisateur testée.
- [ ] Requêtes réseau vérifiées.
- [ ] Logs Supabase contrôlés.

## Portabilité multi-LLM

- [ ] Dépôt GitHub source de vérité.
- [ ] `README.md` explique l’architecture.
- [ ] `AGENTS.md`, `CLAUDE.md` ou équivalent décrit les conventions.
- [ ] `.env.example` sans secrets.
- [ ] Migrations SQL versionnées.
- [ ] Installation locale documentée.
- [ ] Build, lint et tests reproductibles.
- [ ] CI minimale active.
- [ ] Documentation GitHub Pages publiée.
- [ ] Release PDF ou Word archivée à chaque version majeure.

## Sécurité avant publication publique

- [ ] Aucun email personnel visible dans les captures.
- [ ] Aucun UUID utilisateur visible.
- [ ] Aucun Project ID client confidentiel.
- [ ] Aucun token, secret, mot de passe ou chaîne PostgreSQL.
- [ ] Aucun nom de client sans autorisation.
- [ ] Tous les exemples utilisent des placeholders.
- [ ] Les captures ont été recadrées ou floutées.

---

<!-- nav-footer -->

<div align="center">

[← 5. Dépannage](05-depannage.md) &nbsp;·&nbsp; [🏠 Accueil](index.md) &nbsp;·&nbsp; [7. Portabilité multi-LLM →](07-portabilite-multi-llm.md)

</div>
