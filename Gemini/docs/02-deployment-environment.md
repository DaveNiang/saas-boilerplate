# 02 - Environnement de Déploiement

## Vue d'ensemble
Ce document justifie le choix de l'infrastructure d'hébergement sur AWS pour garantir un équilibre parfait entre coût, robustesse, et préparation à la mise à l'échelle (scale-out) sans modification de l'application.

## Détails

### Hébergement Compute (Backend & Frontend)
- **Service** : Amazon ECS sur AWS Fargate (Elastic Container Service).
- **Justification / Rationale** :
  - *Serverless Container* : Fargate permet d'exécuter des conteneurs Docker (notre backend Spring Boot) sans gérer d'instances EC2 sous-jacentes. Réduction drastique de l'overhead DevOps.
  - *Évolutivité* : Permet d'évoluer vers Amazon EKS (Kubernetes) lors de la Phase 3 (lorsque l'entreprise sera plus mature) sans changer la conteneurisation de base.
- Le frontend React/Vite (qui produit des fichiers statiques) sera hébergé sur un bucket **Amazon S3** et distribué mondialement via **Amazon CloudFront** (CDN) pour une latence minimale.

### Base de Données
- **Service** : Amazon RDS for PostgreSQL (Multi-AZ).
- **Justification / Rationale** : Service managé complet offrant des sauvegardes automatiques, de la haute disponibilité (Multi-AZ) et des performances optimales. Indispensable pour un SaaS B2B où la donnée est critique.

### Stockage de Fichiers & Evénements Asynchrones
- **Fichiers** : Amazon S3 (ex: uploads de documents liés aux Contacts, factures générées).
- **Files d'attente (Queues)** : Amazon SQS.
- **Justification / Rationale** : SQS offre un Event Bus ultra-résilient et "serverless" pour découpler les modules (ex: envoi de notification asynchrone). S3 est le standard absolu pour le stockage d'objets.

### Identité et Sécurité
- **Identity Provider** : Déploiement d'un conteneur **Keycloak** (idéalement sur ECS Fargate également, branché sur RDS).
- **Secrets** : AWS Secrets Manager (pour stocker les mots de passe de base de données, clés d'API).

## Tableau comparatif

| Critère | PaaS (Vercel + Railway) | Cloud Provider (AWS) | Choix |
|---------|-------------------------|----------------------|-------|
| Facilité de déploiement initial | ⭐⭐⭐⭐⭐ (Très facile) | ⭐⭐⭐ (Nécessite Terraform) | |
| Coût à grande échelle | ⭐⭐ (Devient vite très cher) | ⭐⭐⭐⭐⭐ (Maîtrisé) | ✓ |
| Conformité & Compliance (B2B) | ⭐⭐⭐ (Limité) | ⭐⭐⭐⭐⭐ (SOC2, HIPAA, ISO) | ✓ |
| Écosystème & Extensibilité | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ (Services infinis) | ✓ |

## Décision finale
L'investissement initial dans la configuration d'**AWS via Terraform (IaC)** est largement justifié pour un SaaS B2B Enterprise. Il garantit une sécurité maximale (VPC privés, isolation des bases de données), des coûts maîtrisables à grande échelle et une conformité requise par les clients entreprise, comparé aux solutions PaaS "magiques" qui cachent l'infrastructure.

## Prochaines étapes
- [ ] Rédiger `03-architecture.md`
