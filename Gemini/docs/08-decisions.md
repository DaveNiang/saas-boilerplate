# 08 - Decision Log (Registre des Décisions)

## Vue d'ensemble
Ce document (Architecture Decision Records - ADR) trace les décisions majeures prises lors de la Phase 1 pour ne pas perdre l'historique du "Pourquoi" au fil du temps.

## Décisions Clés

### ADR-001: Monolithe Modulaire plutôt que Microservices
- **Date** : Phase 1
- **Contexte** : Création d'un SaaS Boilerplate hautement évolutif.
- **Décision** : Utiliser Spring Modulith dans un unique backend Java plutôt qu'un cluster de microservices.
- **Raison** : Les microservices entraînent une forte complexité opérationnelle (déploiement, eventual consistency, monitoring) qui est une suringénierie (overengineering) pour un V1. Le monolithe modulaire permet de garder la simplicité transactionnelle tout en garantissant un code découplé et prêt à être extrait plus tard.

### ADR-002: Séparation de l'Identity Management et du RBAC
- **Date** : Phase 1
- **Contexte** : Gestion de l'authentification et de l'autorisation des utilisateurs.
- **Décision** : Utiliser Keycloak uniquement pour l'Identité globale (AuthN) et gérer le RBAC (Rôles & Permissions) directement dans la base de données de l'application via Spring Security.
- **Raison** : Dans un SaaS B2B Multi-Tenant, un utilisateur peut avoir des rôles différents selon le Workspace (ex: "Admin" dans Workspace A, "Viewer" dans Workspace B). Gérer cette matrice croisée directement dans les rôles Keycloak crée un couplage fort, rigide et complexe à maintenir. L'application métier doit rester maîtresse de ses autorisations locales.

### ADR-003: Séparation entre Workspaces (Technique) et UO/Entités (Métier)
- **Date** : Phase 1
- **Contexte** : Gestion des entreprises clientes, de leurs départements internes et de leurs propres clients.
- **Décision** : Le Workspace reste un conteneur purement technique (Niveau 0). Les Unités Organisationnelles (la hiérarchie interne RH) et les Entités externes (les Contacts, les clients de notre client) sont gérées en tant qu'objets métier (Niveau 1).
- **Raison** : Bien qu'ils représentent tous des "Organisations", fusionner la technique et le métier est dangereux. 
  1. Le Workspace dicte la facturation et la frontière de sécurité absolue (Row Level Security dans Postgres). 
  2. Créer une seule table générique risquerait d'introduire des failles où un contact externe pourrait accidentellement hériter de permissions internes. 
  Le Workspace est la boîte, l'UO et les Entités sont le contenu de la boîte.

### ADR-004: OpenAPI Code Generation
- **Date** : Phase 1
- **Contexte** : Communication entre le Frontend (React) et Backend (Spring).
- **Décision** : Le frontend ne définit pas ses propres interfaces DTO, elles sont générées depuis SpringDoc.
- **Raison** : Type safety garantie de bout en bout et suppression du code boilerplate.
