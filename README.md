# La Méthode Gab-E OPEN — Construire avec Lovable sans lock-in

Documentation publique et réutilisable pour :

- démarrer un projet Lovable avec un **GitHub** et un **Supabase externe** dès le départ ;
- éviter l’activation automatique de Lovable Cloud lorsque ce n’est pas souhaité ;
- récupérer un projet déjà engagé sur Lovable Cloud ;
- restaurer la base, Auth, profils, triggers et politiques RLS ;
- conserver un socle portable vers Claude, ChatGPT, Codex, Cursor, un IDE local, Vercel, Netlify ou une autre infrastructure.

## Format recommandé

Cette documentation est conçue pour être publiée dans un **dépôt GitHub public séparé du dépôt de l’application**, via **GitHub Pages**. Le dépôt de l’application peut rester privé.

## Publication rapide

1. Créez un dépôt GitHub public, par exemple `gab-e-lovable-portability-guide`.
2. Copiez le contenu de ce dossier à la racine du dépôt.
3. Poussez sur la branche `main`.
4. Ouvrez `Settings > Pages`.
5. Choisissez `Deploy from a branch`.
6. Sélectionnez `main` puis `/docs`.
7. Enregistrez.

GitHub Pages publiera le site à une adresse proche de :

```text
https://VOTRE-COMPTE.github.io/gab-e-lovable-portability-guide/
```

## Sécurité

Ne publiez jamais :

- un dump `.backup`, `.sql`, `.sql.gz` ;
- un fichier `.env` réel ;
- une `service_role`, une secret key ou un mot de passe PostgreSQL ;
- une chaîne de connexion contenant un mot de passe ;
- une capture contenant des emails, UUID, clés, noms de clients ou données métier non masquées.

## Contenu

- [Accueil](docs/index.md)
- [Méthode Gab-E OPEN](docs/01-methode-gab-e-open.md)
- [Prévenir Lovable Cloud](docs/02-prevenir-lovable-cloud.md)
- [Migration de rattrapage](docs/03-migration-rattrapage.md)
- [Bascule et validation](docs/04-bascule-validation.md)
- [Dépannage](docs/05-depannage.md)
- [Checklists](docs/06-checklists.md)
- [Portabilité vers d’autres LLM](docs/07-portabilite-multi-llm.md)
- [Cas pratique : projet CRM](docs/08-cas-pratique-crm.md)
- [Sources officielles](docs/09-sources-officielles.md)

## Version

Version initiale : **0.1 — 19 juillet 2026**.

Les interfaces SaaS évoluent rapidement. Chaque procédure doit être revérifiée avant une suppression, une migration ou une bascule de production.
