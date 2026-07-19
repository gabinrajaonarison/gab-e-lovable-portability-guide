[← Retour à l'accueil](index.md)

# Bascule du frontend & tests de validation

## Bascule du frontend

Une fois la nouvelle base vérifiée, pointez l'application vers **votre** Supabase.

1. Mettez à jour les variables d'environnement :

   ```env
   VITE_SUPABASE_URL=https://<PROJECT_REF>.supabase.co
   VITE_SUPABASE_PUBLISHABLE_KEY=<PUBLISHABLE_KEY>
   ```

2. Mettez à jour les mêmes variables **chez votre hébergeur** (par ex. Vercel) —
   pour la production, la préproduction et le développement.

3. Redéployez.

> 🔁 Gardez l'ancienne configuration **de côté** (dans un fichier non commité) le
> temps de valider. En cas de souci, vous pouvez revenir en arrière vite.

> 🔒 Rappel : seule la clé **publique** va dans le frontend/hébergeur. La clé
> `service_role` reste un **secret serveur**.

## Tests de validation

Déroulez ces tests **après** la bascule, en conditions réelles :

- [ ] **Inscription** d'un nouveau compte → l'utilisateur apparaît dans
      `auth.users` **et** un profil est créé (trigger OK).
- [ ] **Connexion** avec un compte existant migré.
- [ ] **Mot de passe oublié** → email reçu → réinitialisation → connexion.
- [ ] **Lecture** des données d'un utilisateur : il ne voit **que** ce qu'il doit
      voir (RLS OK).
- [ ] **Écriture** : créer/modifier une donnée, vérifier en base.
- [ ] **Accès refusé** attendu : tenter d'accéder à la donnée d'un autre
      utilisateur → refus.
- [ ] **Fonctions serveur / Edge Functions** : appelées avec les bons secrets.
- [ ] **Aucune erreur** dans la console navigateur ni dans les logs serveur.

Si tous ces tests passent, la migration est validée. **Alors seulement**, vous
pouvez exporter une dernière sauvegarde de l'ancienne base et la supprimer.

[← Vérifications](03-verifications.md) · [Suivant : Dépannage & checklist →](05-depannage-checklist.md)
