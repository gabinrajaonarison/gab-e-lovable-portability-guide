[← Retour à l'accueil](index.md)

# Dépannage & checklist opérationnelle

## Dépannage des erreurs courantes

### « Invalid login credentials » après migration
Le compte existe mais l'authentification échoue. Vérifiez :
- que l'utilisateur est bien dans `auth.users` ;
- qu'il a une identité dans `auth.identities` (fournisseur `email`) ;
- que les comptes n'ont pas juste été copiés « en table » sans passer par le
  service Auth. Au besoin, recréez les comptes via l'API Admin et invitez les
  utilisateurs à réinitialiser leur mot de passe.

### Un nouvel inscrit n'a pas de profil
Le **trigger** de création de profil sur `auth.users` n'a pas été restauré.
Recréez la fonction + le trigger (voir [Vérifications](03-verifications.md#4-triggers)).

### « permission denied for table … » / données invisibles
Souvent la **RLS** : table avec RLS activée mais **sans policy** adaptée. Ajoutez
les policies, ou vérifiez que la requête est faite avec le bon rôle
(clé publique côté client, jamais `service_role` dans le navigateur).

### Le frontend pointe encore vers l'ancienne base
Les variables d'environnement n'ont pas été mises à jour **chez l'hébergeur**
(elles le sont en local mais pas en production). Mettez-les à jour pour tous les
environnements, puis redéployez.

### Restauration `pg_restore` : erreurs de propriétaire/rôle
Ajoutez `--no-owner --no-privileges`. Les propriétaires et `GRANT` d'origine
n'existent pas dans le nouveau projet ; vous redéfinirez la sécurité via la RLS.

### Doublons après une restauration rejouée
Vous avez rejoué le dump sur une base non vide. Repartez d'une base propre, ou
utilisez `--clean --if-exists` en connaissance de cause.

---

## Checklist opérationnelle

**Avant de construire**
- [ ] Code sur **mon** dépôt GitHub, synchronisé.
- [ ] **Mon** projet Supabase connecté (URL + clé publique en variables d'env).
- [ ] `service_role` **jamais** dans le front ni dans un dépôt.

**Avant de migrer**
- [ ] Export/dump de la base source réalisé et **conservé**.
- [ ] Nouveau projet Supabase créé, identifiants notés en lieu sûr.

**Pendant la migration**
- [ ] Restauration `pg_restore`/`psql` réussie.
- [ ] `auth.users`, `auth.identities`, profils, triggers vérifiés.
- [ ] RLS activée + policies rejouées sur toutes les tables publiques.

**Après la bascule**
- [ ] Variables d'env mises à jour **partout** (local + hébergeur, tous envs).
- [ ] Tous les [tests de validation](04-bascule-et-tests.md#tests-de-validation)
      passent.
- [ ] Ancienne base conservée en lecture le temps de valider.
- [ ] **Puis seulement** : dernière sauvegarde + suppression de l'ancienne base.

[← Bascule & tests](04-bascule-et-tests.md) · [Suivant : Lovable → IA/IDE →](06-lovable-vers-ia.md)
