# Gab-E — Guide de portabilité Lovable → Supabase

Version publique **0.1**, anonymisée. Méthode **KISS + Ownership Gate** pour garder
la maîtrise de son code (GitHub) et de ses données (Supabase) avec un projet
démarré sous Lovable, et migrer sereinement vers un Supabase que l'on possède.

📖 **Documentation (GitHub Pages) :** une fois Pages activé, le site est publié à
partir du dossier [`/docs`](docs/index.md).

## Sommaire

- [Accueil](docs/index.md)
- [Démarrage propre : GitHub + Supabase](docs/01-demarrage-propre.md)
- [Migration Lovable → Supabase + `pg_restore`](docs/02-migration.md)
- [Vérifications Auth / RLS](docs/03-verifications.md)
- [Bascule du frontend & tests](docs/04-bascule-et-tests.md)
- [Dépannage & checklist](docs/05-depannage-checklist.md)
- [Lovable → Claude / ChatGPT / Codex / Cursor / IDE local](docs/06-lovable-vers-ia.md)
- [Sources & vérification](docs/07-sources.md)

## Activer GitHub Pages

`Settings → Pages → Deploy from a branch → main → dossier /docs → Save`.

## Anonymisation

Aucun email, UUID, clé, mot de passe ni identifiant de projet réel. Uniquement des
placeholders (`<PROJECT_REF>`, `<DB_PASSWORD>`, `<GITHUB_USER>`…).

## Licence

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — réutilisation libre avec attribution.
