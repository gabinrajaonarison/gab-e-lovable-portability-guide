[← Retour à l'accueil](index.md)

# Migration Lovable Cloud → Supabase (rattrapage)

Vous n'avez pas connecté votre Supabase au départ et une base managée s'est
activée ? Voici la procédure de rattrapage. Le principe : **exporter**, **recréer
chez vous**, **vérifier**, **basculer**, **puis** nettoyer.

> ⚠️ **Ne supprimez rien** tant que la nouvelle base n'est pas vérifiée et que le
> frontend n'y est pas branché avec succès. Gardez l'ancienne en lecture le temps
> de valider.

## Vue d'ensemble

```
1. Exporter les données (dump PostgreSQL)
2. Créer un projet Supabase à soi
3. Restaurer le dump (pg_restore / psql)
4. Vérifier Auth, profils, triggers, RLS
5. Basculer le frontend vers le nouveau Supabase
6. Tester
7. Seulement ensuite : exporter/supprimer la base managée
```

## 1. Exporter les données

Selon la source, vous obtiendrez soit un fichier **`.sql`** (texte), soit un
**dump custom** PostgreSQL (`.dump` / `.backup`).

- Depuis Supabase, un export standard se fait avec `pg_dump` sur la chaîne de
  connexion du projet.
- Depuis une base managée par un outil tiers, utilisez la fonction d'**export**
  proposée par l'outil (voir sa doc), qui produit généralement un dump PostgreSQL.

Exemple d'export (dump custom, recommandé pour `pg_restore`) :

```bash
pg_dump \
  "postgresql://postgres.<SRC_REF>:<SRC_DB_PASSWORD>@<SRC_HOST>:5432/postgres" \
  -Fc -f sauvegarde.dump
```

- `-Fc` = format *custom* (compressé, restaurable sélectivement avec `pg_restore`).
- Pour un export en SQL simple, remplacez par `-f sauvegarde.sql` (sans `-Fc`).

## 2. Créer un projet Supabase à soi

Créez le projet sur [supabase.com](https://supabase.com), notez :

- `Project URL` : `https://<PROJECT_REF>.supabase.co`
- Le **mot de passe de la base** (défini à la création) : `<DB_PASSWORD>`
- La chaîne de connexion (onglet *Connect*).

## Restauration avec `pg_restore`

Pour un dump **custom** (`-Fc`) :

```bash
pg_restore \
  --no-owner --no-privileges \
  -d "postgresql://postgres.<PROJECT_REF>:<DB_PASSWORD>@<HOST>:5432/postgres" \
  sauvegarde.dump
```

Options utiles :

- `--no-owner` : ne réattribue pas les propriétaires d'origine (souvent absents
  dans le nouveau projet).
- `--no-privileges` : ignore les `GRANT` d'origine (vous re-définirez la RLS).
- `--schema=public` : pour restaurer uniquement le schéma applicatif si besoin.
- `--clean --if-exists` : pour rejouer proprement sur une base non vide (à manier
  avec prudence).

Pour un dump **SQL** simple, utilisez `psql` plutôt que `pg_restore` :

```bash
psql "postgresql://postgres.<PROJECT_REF>:<DB_PASSWORD>@<HOST>:5432/postgres" \
  -f sauvegarde.sql
```

> 💡 Le schéma `auth` de Supabase est géré par le service Auth. Restaurez en
> priorité votre schéma **`public`** (vos tables applicatives). Pour les comptes
> utilisateurs, voir [Vérifications Auth](03-verifications.md).

[← Démarrage propre](01-demarrage-propre.md) · [Suivant : Vérifications →](03-verifications.md)
