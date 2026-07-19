[← Retour à l'accueil](index.md)

# Lovable → Claude, ChatGPT, Codex, Cursor ou IDE local

Une fois le code sur GitHub et les données sur votre Supabase, vous n'êtes plus
lié à un seul environnement d'édition. Vous pouvez continuer avec l'outil qui vous
convient.

## Le principe

Votre projet est **standard** (code Git + base PostgreSQL). N'importe quel
assistant ou IDE capable de lire un dépôt peut prendre le relais.

```
GitHub (le code)  +  Supabase (les données)
        │
        └── éditez avec : Lovable, Cursor, un IDE local,
            ou un assistant (Claude, ChatGPT, Codex…) branché sur le dépôt
```

## Passer à un IDE local

```bash
git clone https://github.com/<GITHUB_USER>/<REPO>.git
cd <REPO>
# copiez vos variables d'environnement dans un fichier .env local (non commité)
npm install
npm run dev
```

- Gardez `.env` (et `.env.local`) dans le **`.gitignore`**.
- Ne commitez jamais de clés. Utilisez les secrets de votre hébergeur pour la prod.

## Passer à Cursor / un assistant IA

- **Cursor** : *Open folder* sur le dépôt cloné — il travaille sur vos fichiers
  locaux, versionnés par Git.
- **Assistants (Claude, ChatGPT, Codex…)** : reliez-les au dépôt (ou copiez le
  contexte). Comme le code est standard, ils lisent et modifient sans dépendance
  propriétaire.

## Aller-retour avec Lovable

Tant que la **synchronisation GitHub** est active, vous pouvez faire des
allers-retours : éditer en local ou avec un assistant, pousser sur GitHub, et
retrouver les changements dans Lovable (et inversement). C'est là tout l'intérêt
d'avoir franchi l'**Ownership Gate** tôt.

> Note d'anonymisation : les exemples ci-dessus (`<GITHUB_USER>`, `<REPO>`) sont
> des placeholders. Un éventuel cas réel a été retiré de cette version publique.

[← Dépannage & checklist](05-depannage-checklist.md) · [Suivant : Sources →](07-sources.md)
