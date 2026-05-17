# 03 - Architecture Globale

## Vue d'ensemble
L'architecture de l'application suit le pattern du **Monolithe Modulaire** orchestré par **Spring Modulith**. L'infrastructure est déployée de manière scalable sur AWS.

## Détails

### 1. Monolithe Modulaire (Software Architecture)
Au lieu de découper l'application en 10 microservices dès le premier jour, nous regroupons tous les Bounded Contexts (Workspaces, Contacts, Calendrier) dans un seul backend (une seule JVM Java 21). 
- **Isolation garantie** : Spring Modulith agit comme un "garde-fou". Il empêche (via des tests ArchUnit) qu'une classe du module "Calendrier" importe une classe interne du module "Contacts".
- **Communication Inter-Modules** : Les modules communiquent via des appels d'API internes explicites ou de manière asynchrone (Spring Application Events).
- **Avantage** : Facilité de déploiement (1 seul conteneur) avec la rigueur d'une architecture microservice. Facile à extraire plus tard.

### 2. AWS Cloud Architecture
- L'utilisateur final accède au CDN (CloudFront) pour le Frontend React.
- L'API est servie par un Application Load Balancer (ALB) qui répartit la charge sur nos instances de conteneurs dans AWS ECS Fargate.
- La base de données est gérée par Amazon RDS (PostgreSQL).
- L'Identity Provider (Keycloak) tourne dans son propre conteneur Fargate ou instance EC2.
- Les événements complexes sont routés vers SQS pour ne pas saturer l'API.

## Diagrammes

Pour visualiser l'infrastructure et les flux de données, veuillez vous référer aux fichiers Mermaid situés dans le répertoire `docs/diagrams/` :
- `docs/diagrams/architecture-diagram.md`
- `docs/diagrams/data-flow-diagram.md`

## Décision finale
L'adoption de **Spring Modulith** est la décision architecturale la plus impactante. Elle protège contre le phénomène de "plat de spaghettis" typique des monolithes traditionnels, tout en évitant l'enfer opérationnel d'un cluster Kubernetes rempli de 15 microservices pour un projet au stade initial.

## Prochaines étapes
- [ ] Construire la CI/CD pour supporter cette architecture de conteneurs.
