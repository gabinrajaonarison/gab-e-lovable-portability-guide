# Guide de portabilité Lovable → Supabase

**Version publique 0.1 — anonymisée.**
Garder la maîtrise de son code et de ses données quand on démarre un projet avec
Lovable, et pouvoir migrer sereinement vers un Supabase que l'on possède.

> ⚠️ **Anonymisation.** Cette version publique ne contient **aucun** email, UUID,
> clé d'API, mot de passe ni identifiant de projet réel. Partout où une valeur
> personnelle serait attendue, un **placeholder** est utilisé, par ex.
> `<PROJECT_REF>`, `<DB_PASSWORD>`, `<GITHUB_USER>`.

> ℹ️ **Statut.** Document de travail v0.1. Les procédures internes aux outils tiers
> (Lovable, Supabase, GitHub) évoluent : vérifiez toujours la **documentation
> officielle** citée dans [Sources](07-sources.md) avant d'exécuter une étape
> sensible (export, suppression, restauration de base).

---

## La méthode en une phrase

> **KISS + Ownership Gate** : garder l'architecture simple, et ne jamais avancer
> sans être sûr de **posséder et contrôler** son code (GitHub) et ses données
> (Supabase).

## Sommaire

1. [Méthode KISS + Ownership Gate](01-demarrage-propre.md#méthode)
2. [Démarrage propre : GitHub + Supabase dès le début](01-demarrage-propre.md)
3. [Empêcher l'activation automatique de Lovable Cloud](01-demarrage-propre.md#empêcher-lactivation-automatique-de-lovable-cloud)
4. [Migration Lovable Cloud → Supabase (rattrapage)](02-migration.md)
5. [Sauvegardes PostgreSQL : `pg_restore`](02-migration.md#restauration-avec-pg_restore)
6. [Vérifications Auth, `auth.identities`, profils, triggers, RLS](03-verifications.md)
7. [Bascule du frontend](04-bascule-et-tests.md#bascule-du-frontend)
8. [Tests de validation](04-bascule-et-tests.md#tests-de-validation)
9. [Dépannage des erreurs courantes](05-depannage-checklist.md)
10. [Checklist opérationnelle](05-depannage-checklist.md#checklist-opérationnelle)
11. [Lovable → Claude, ChatGPT, Codex, Cursor ou IDE local](06-lovable-vers-ia.md)
12. [Sources officielles et dates de vérification](07-sources.md)

---

## À qui s'adresse ce guide

- Vous avez démarré (ou allez démarrer) un projet avec **Lovable**.
- Vous voulez éviter de vous retrouver **enfermé** dans une base de données que
  vous ne maîtrisez pas.
- Vous préférez un stack **portable** : le code sur **GitHub**, les données sur un
  **Supabase** que vous possédez, un hébergement de votre choix.

## Ce que vous NE trouverez pas ici

- Aucune donnée réelle ni secret.
- Aucun cas client identifiable (les exemples sont génériques et anonymisés).
- Aucune promesse : les outils tiers changent — ce guide pointe vers leurs docs.

---

*Publié via GitHub Pages. Contributions bienvenues via issues/PR.*
