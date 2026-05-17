# 04 - Tech Stack & Technologies

## Vue d'ensemble
Ce document définit la pile technologique finale (backend, frontend, infrastructure) choisie pour le SaaS Boilerplate. L'objectif principal est de garantir une architecture "Day One Operational", qui soit robuste, sécurisée, et hautement évolutive grâce au modèle du Monolithe Modulaire.

## Détails

### 1. Backend Core
- **Runtime** : Java 21 (virtual threads activés)
- **Framework** : Spring Boot 3.3.x
- **Architecture** : Spring Modulith
- **Sécurité** : Spring Security 6 + OAuth2 Resource Server
- **Validation** : Jakarta Validation + Bean Validation
- **Documentation API** : SpringDoc OpenAPI 3
- **Build & Packaging** : Maven multi-modules
- **Justification / Rationale** : Le choix de Java 21 et Spring Boot assure des performances optimales (concurrence via virtual threads) et une fiabilité d'entreprise. Spring Modulith force l'isolation des Bounded Contexts, ce qui empêche le couplage fort entre les différents modules (Workspaces, Contacts, Calendrier) tout en gardant la simplicité de déploiement d'un monolithe.

### 2. Base de Données, ORM & Cache
- **Base de Données** : PostgreSQL 15 (sur AWS RDS)
- **ORM** : Spring Data JPA + Hibernate 6
- **Migrations** : Flyway
- **Multi-tenant** : Row Level Security (RLS) PostgreSQL
- **Cache/Queue** : Redis (ElastiCache) — *Prévu pour la V2*
- **Justification / Rationale** : PostgreSQL est le standard absolu pour les données relationnelles complexes (ex: lien récursif des UO). L'utilisation native du RLS garantit qu'aucune fuite de données inter-tenant n'est possible à la couche la plus basse de l'application. Flyway assure la traçabilité des schémas.

### 3. Frontend & UI
- **Framework** : React 18 + TypeScript + Vite
- **Typage API** : openapi-generator (généré depuis le backend SpringDoc)
- **UI & Styling** : Shadcn/ui + Tailwind CSS
- **State Management** : React Query
- **Justification / Rationale** : Vite et React 18 offrent une expérience développeur excellente et des temps de build ultra-rapides. L'utilisation d'openapi-generator garantit que le typage TypeScript du frontend est toujours synchronisé avec les DTOs Java du backend. Tailwind et Shadcn permettent de construire des interfaces premium, responsives et customisables très rapidement.

### 4. Infrastructure AWS & DevOps
- **Compute** : ECS Fargate (V1) → Évolutif vers EKS (V3+)
- **Events / Asynchrone** : SQS (AWS SDK v2)
- **Fichiers** : S3 + CloudFront
- **Identité** : Keycloak (Container séparé)
- **Sécurité des secrets** : AWS Secrets Manager
- **Monitoring** : CloudWatch + Datadog
- **Infrastructure as Code** : Terraform (modules réutilisables)
- **Justification / Rationale** : AWS ECS Fargate évite la gestion des serveurs tout en offrant une conteneurisation robuste. Terraform permet de documenter et de reproduire l'infrastructure. SQS est idéal pour l'Event Bus du monolithe modulaire.

### 5. CI/CD & Qualité
- **Pipeline** : GitHub Actions
- **Registry** : ECR
- **Déploiement** : ECS Rolling Update → Blue/Green prévu en V2
- **Qualité & Sécurité** : SonarQube + Snyk
- **Tests** : JUnit 5 + Testcontainers + Mockito
- **Justification / Rationale** : Testcontainers permet des tests d'intégration réels sur une base PostgreSQL éphémère Docker, garantissant que les politiques RLS fonctionnent parfaitement avant de déployer sur AWS via GitHub Actions.

## Décision finale
L'architecture validée est une stack Enterprise Java moderne combinée à un frontend React TypeScript, déployée sur AWS Fargate. Ce choix privilégie fortement la **sécurité des données (RLS)**, la **qualité architecturale (Modulith)** et la **maintenabilité à long terme**.

## Prochaines étapes
- [ ] Rédiger `01-composants.md`
- [ ] Rédiger `05-database-schema.md`
- [ ] Rédiger `06-project-structure.md`
