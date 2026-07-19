# Politique de sécurité documentaire

Cette documentation ne doit jamais contenir :

- tokens ;
- secrets ;
- mots de passe ;
- dumps de base ;
- `.env` réel ;
- emails personnels non autorisés ;
- UUID utilisateurs ;
- données clients ;
- captures non anonymisées.

Si un secret est publié :

1. le révoquer immédiatement ;
2. générer une nouvelle valeur ;
3. retirer le secret du dépôt ;
4. nettoyer l’historique Git si nécessaire ;
5. documenter l’incident sans republier la valeur.
