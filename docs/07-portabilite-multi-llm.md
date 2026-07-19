---
layout: default
title: Portabilité multi-LLM
---

# 7. Passer de Lovable à Claude, ChatGPT, Codex, Cursor ou un IDE local

## Le principe

Une fois GitHub et Supabase externalisés, Lovable n’est plus l’unique environnement capable de faire évoluer le projet.

Le même dépôt peut être utilisé par :

- Lovable ;
- Claude Code ou Claude Desktop avec un environnement de développement ;
- ChatGPT / Codex ;
- Cursor ;
- VS Code ;
- une équipe de développeurs ;
- une CI/CD externe.

## GitHub devient le contrat commun

Le dépôt doit contenir :

```text
README.md
AGENTS.md ou CLAUDE.md
src/
supabase/migrations/
.env.example
package.json
scripts de test
```

Chaque agent IA doit recevoir les mêmes règles :

- architecture ;
- conventions ;
- commandes ;
- périmètre ;
- contraintes de sécurité ;
- interdictions de migrations destructrices ;
- critères de réussite.

## Exemple de fichier `AGENTS.md`

```markdown
# Project rules

## Architecture
- Frontend: React / TypeScript.
- Backend: external Supabase project.
- Database schema: versioned in supabase/migrations.

## Security
- Never expose service_role or database passwords.
- All public tables require RLS.
- Never modify auth schema directly without an approved migration plan.

## Change policy
- Present destructive changes before execution.
- Do not rename tables or columns outside the requested scope.
- Run build and tests before completion.
```

## Cloner et travailler localement

```bash
git clone https://github.com/OWNER/REPOSITORY.git
cd REPOSITORY
npm install
cp .env.example .env.local
npm run dev
```

Les vraies valeurs de `.env.local` restent hors Git.

## Prompt de transfert vers un autre agent

```text
CONTEXT
Ce projet a été initialement construit avec Lovable, mais GitHub est désormais la source de vérité et le backend est un Supabase externe.

TASK
Réalise la fonctionnalité demandée dans le dépôt existant.

GUIDELINES
Respecte l’architecture, les migrations versionnées, les policies RLS et les conventions décrites dans AGENTS.md.
Exécute build, lint et tests.

CONSTRAINTS
Ne modifie pas les secrets.
Ne crée pas un nouveau backend.
Ne remplace pas Supabase.
Ne lance pas de migration destructive sans validation.
Ne touche pas aux fichiers hors périmètre.
```

## Déployer hors Lovable

Le frontend peut ensuite être déployé sur :

- Vercel ;
- Netlify ;
- Cloudflare Pages ;
- un VPS ;
- un conteneur ;
- une infrastructure d’entreprise.

Le backend Supabase reste le même tant que les variables d’environnement pointent vers le même projet.

## Stratégie recommandée

1. Lovable pour accélérer le prototypage et les itérations UI.
2. GitHub comme source de vérité.
3. Supabase externe comme backend contrôlé.
4. Claude ou ChatGPT pour les audits, refactorings et tâches ciblées.
5. CI/CD pour valider build, lint et tests.
6. GitHub Pages pour documenter la méthode et les décisions.

---

<!-- nav-footer -->

<div align="center">

[← 6. Checklists](06-checklists.md) &nbsp;·&nbsp; [🏠 Accueil](index.md) &nbsp;·&nbsp; [8. Cas pratique ProspectFlow →](08-cas-pratique-prospectflow.md)

</div>
