# Publier la documentation sur GitHub Pages

## Recommandation

Créez un dépôt public séparé du dépôt privé de votre application :

```text
gab-e-lovable-portability-guide
```

Ainsi, la documentation est publique sans exposer le code ou les données du projet ProspectFlow.

## Étapes

1. Créer le dépôt public sur GitHub.
2. Décompresser ce package.
3. Envoyer les fichiers à la racine du dépôt.
4. Ouvrir `Settings > Pages`.
5. Dans `Build and deployment`, choisir `Deploy from a branch`.
6. Sélectionner la branche `main`.
7. Sélectionner le dossier `/docs`.
8. Cliquer sur `Save`.
9. Attendre la publication.

## URL attendue

```text
https://VOTRE-COMPTE.github.io/gab-e-lovable-portability-guide/
```

## Domaine personnalisé facultatif

Vous pourrez ensuite utiliser un sous-domaine, par exemple :

```text
docs.gab-e.app
method.gab-e.app
```

Ajoutez d’abord le domaine dans GitHub Pages, puis configurez le DNS demandé par GitHub.

## Mise à jour

Chaque modification poussée sur `main/docs` republie la documentation.
