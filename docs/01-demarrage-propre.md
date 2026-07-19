[← Retour à l'accueil](index.md)

# Démarrage propre : GitHub + Supabase dès le début

## Méthode

### KISS

*Keep It Simple.* Un stack simple et standard est un stack portable. Concrètement :

- Un framework front classique (React/Vite ou équivalent).
- Une base **PostgreSQL via Supabase** (standard, exportable).
- Le code versionné sur **GitHub**.
- Un hébergement au choix (par ex. Vercel).

Moins il y a de « magie » propriétaire, plus il est facile de partir ailleurs.

### Ownership Gate

Un **point de contrôle** que l'on se fixe **avant** de construire des fonctionnalités :

> Je ne continue pas tant que je ne possède pas **mon code** (dépôt GitHub à moi)
> **et mes données** (projet Supabase à moi), avec un accès complet aux deux.

Tant que cette « porte » n'est pas franchie, on reste dépendant d'un outil tiers
pour ce qui compte le plus : le code source et la base de données.

---

## Connecter GitHub dès le démarrage

La synchronisation GitHub fournit une **copie** et une **synchronisation
bidirectionnelle** du code : vous éditez dans l'outil ou en local, tout reste
aligné.

1. Depuis le projet Lovable, ouvrez l'intégration **GitHub** et connectez votre
   compte.
2. Laissez Lovable créer (ou reliez) un dépôt sous **votre** compte
   `<GITHUB_USER>`.
3. Vérifiez côté GitHub que le dépôt existe et se met à jour à chaque changement.

> Objectif : à tout moment, `git clone` doit suffire pour récupérer l'intégralité
> du code sur votre machine.

## Connecter votre propre Supabase dès le démarrage

Plutôt que de laisser une base « managée » s'activer par défaut, connectez **votre**
projet Supabase :

1. Créez un projet sur [supabase.com](https://supabase.com) (il vous appartient).
2. Récupérez son `Project URL` et sa **clé publique** (anon/publishable).
3. Renseignez-les dans les variables d'environnement du projet :

   ```env
   VITE_SUPABASE_URL=https://<PROJECT_REF>.supabase.co
   VITE_SUPABASE_PUBLISHABLE_KEY=<PUBLISHABLE_KEY>
   ```

> 🔒 **Ne mettez jamais** la clé `service_role` dans le frontend ni dans un dépôt
> public. Elle contourne toutes les règles RLS. Elle reste côté serveur
> (fonctions/Edge Functions, secrets).

---

## Empêcher l'activation automatique de Lovable Cloud

L'idée est simple : **occuper la place** avant qu'une base managée ne s'active
d'elle-même.

- **Connectez votre Supabase tôt** (ci-dessus) : la source de données est déjà la
  vôtre.
- **Vérifiez les permissions Cloud** dans les réglages du projet Lovable et
  désactivez l'activation automatique si l'option existe.
- **Gardez GitHub synchronisé** : même si une base managée s'activait, votre code
  resterait intact et déplaçable.

> Les emplacements exacts de ces réglages changent avec les versions de l'outil.
> Reportez-vous à la [documentation Lovable](07-sources.md) au moment où vous le
> faites.

[← Accueil](index.md) · [Suivant : Migration Lovable → Supabase →](02-migration.md)
