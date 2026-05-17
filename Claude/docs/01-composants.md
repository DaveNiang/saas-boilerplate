# 01 - Components & Modules

## Vue d'ensemble
Ce document définit les briques logiques (composants) à assembler pour former le SaaS Boilerplate complet, réparties sur 4 versions successives. Il sépare les composants purement techniques des composants métiers pour la fondation.

---

## Phase 1 (V1 - Foundation)

### 1. Technical Base Components
- **Identity & Authentication Management** : Gérer le "Qui es-tu ?" (SSO, OAuth2, gestion des sessions, MFA). La responsabilité est déléguée à Keycloak.
- **Role-Based Access Control (RBAC)** : Gérer le "Que peux-tu faire ?". Autorisation granulaire basée sur les rôles de l'utilisateur au sein d'un contexte spécifique.
- **Workspace & Tenant Management** : Conteneur racine technique pour isoler les données. Un Workspace représente l'abonnement du client SaaS. C'est la clé de voûte de la sécurité Multi-Tenant (Row Level Security).
- **Audit Trail & Logging** : Traçabilité immuable (Qui a fait quoi, quand et depuis quelle IP). Fonctionnalité essentielle pour la compliance.

### 2. Business Components
- **Organizational Unit & Entity Management** : Référentiel central gérant la hiérarchie interne du client (Unités Organisationnelles : Départements, Services) ainsi que les entités externes (Clients du client, Fournisseurs, Contacts physiques).
- **Communication & Notifications** : Distribuer l'information (Email, SMS, in-app) de manière asynchrone sans bloquer les processus métiers (Event-driven).

---

## Phase 2 (V2 - Business Core)

Ajout des briques nécessaires pour générer des revenus et supporter les opérations quotidiennes.

- **Calendar & Scheduling** : Modéliser le temps. Gérer les disponibilités, la récurrence, les créneaux et les conflits pour des objets métiers.
- **Invoicing & Payments** : Génération de factures, gestion des taxes, intégration Stripe (paiements et abonnements).
- **Resource Management** : Suivi de toute ressource physique ou logique (véhicules, équipements, salles) et gestion de leur cycle de vie/maintenance.
- **Task & Workflow System** : Création, assignation, et suivi des tâches (Vues Kanban, Listes) avec statuts de progression.

---

## Phase 3 (V3 - Operations & Extensibility)

Ajout des composants d'analyse, d'extensibilité et de productivité avancée.

- **Inventory & Stock Management** : Suivi des stocks, alertes de réapprovisionnement, historique des mouvements.
- **Reports & Analytics** : Tableaux de bord personnalisables, KPIs, et exports de rapports.
- **Document & File Management** : Upload sécurisé, stockage cloud (S3), versioning de fichiers et gestion des permissions d'accès.
- **API & Integrations** : Exposition d'une API REST publique documentée, gestion de Webhooks, clés d'API.
- **Advanced Security & Compliance** : Chiffrement granulaire des données sensibles, politiques de rétention (GDPR tooling).

---

## Phase 4 (V4 - Polish & Distribution)

Finition du produit pour qu'il soit pleinement réutilisable comme Boilerplate ultime.

- **Web & Mobile UI System** : Interface utilisateur cohérente, PWA, accessibilité complète, et mode hors ligne (optionnel).
- **Settings & Customization** : Moteur de templates personnalisables, paramètres globaux avancés (marque blanche), et custom fields pour les entités métier.
