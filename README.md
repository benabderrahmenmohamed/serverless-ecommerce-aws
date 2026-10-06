# Serverless E-Commerce & Order Processing Platform

Plateforme e-commerce distribuée avec traitement de commandes événementiel, construite sur AWS (projet de semestre — Architecture Cloud AWS, Projet 2).

## Objectif

Le catalogue, le panier, les commandes, le stock et les notifications sont pris en charge par des composants serverless distincts et faiblement couplés. La valeur du projet réside dans le traitement événementiel des commandes et la gestion cohérente des stocks.

## Architecture

| Couche | Service AWS | Rôle |
| --- | --- | --- |
| Entrée API | API Gateway | Points de terminaison REST (catalogue, panier, commandes) |
| Logique métier | AWS Lambda | Produits, utilisateurs, panier, commandes, inventaire |
| Données structurées | Amazon DynamoDB | Produits, utilisateurs, panier, commandes, inventaire |
| Images produits | Amazon S3 | Stockage des images, déclenchement du traitement événementiel |
| Supervision | Amazon CloudWatch | Journaux, métriques, alarmes |

Flux de commande : le client passe une commande → la Lambda de commande l'écrit dans DynamoDB → cette écriture déclenche, indépendamment, la mise à jour de l'inventaire et la génération d'une notification.

### Contraintes AWS Academy Learner Lab

- Toutes les ressources utilisent `LabRole` (aucun rôle IAM personnalisé).
- Région du projet : `us-east-1`.
- Architecture 100 % serverless pour persister entre les sessions du lab.

## Acteurs

| Rôle | Accès autorisé |
| --- | --- |
| `CLIENT` | Catalogue en lecture, panier et commandes personnels |
| `VENDEUR` | Produits et inventaire qui lui appartiennent, commandes associées |
| `ADMIN` | Ensemble des ressources du système |

## Fonctionnalités clés

- Catalogue avec recherche et filtres (catégorie, prix, disponibilité)
- Panier persistant par client
- Décrémentation atomique du stock (écritures conditionnelles DynamoDB), jamais sous zéro
- Machine à états de la commande : `PENDING → CONFIRMED → PROCESSING → SHIPPED → DELIVERED`, avec `CANCELLED` possible avant l'expédition
- Téléversement d'images vers S3 et traitement déclenché par événement
- Tableau de bord analytique administrateur
- Stratégie de gestion des défaillances documentée et démontrée

## Structure du dépôt

```
src/        Fonctions Lambda (une par composant)
scripts/    Scripts Boto3 (déploiement, démonstration, simulateur de charge)
tests/      Tests unitaires, tests d'API, tests de défaillance
docs/       Livrables et documentation d'architecture
```

## Livrables

| # | Semaines | Contenu |
| --- | --- | --- |
| 1 | 1–2 | Cahier des exigences |
| 2 | 3–4 | Architecture AWS + vérification Learner Lab |
| 3 | 5–6 | Base de données + API |
| 4 | 7–8 | Premier prototype fonctionnel |
| 5 | 9–10 | Intégration AWS complète |
| 6 | 11–12 | Traitement événementiel de bout en bout |
| 7 | 13–14 | Tests et optimisation |
