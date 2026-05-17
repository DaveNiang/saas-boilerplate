# 07 - API Design & Conventions

## Vue d'ensemble
Ce document détaille les patterns RESTful utilisés pour concevoir les contrôleurs du backend.

## Détails

### 1. Conventions RESTful Standard
- Les URLs utilisent toujours des noms au pluriel (`/contacts`, `/workspaces`).
- L'utilisation des verbes HTTP est stricte : `GET` (Lecture), `POST` (Création), `PUT` (Mise à jour complète), `PATCH` (Mise à jour partielle), `DELETE` (Suppression).

### 2. Rooting autour du Workspace
Comme abordé dans l'architecture, chaque appel API (sauf l'Auth globale) doit s'effectuer dans le contexte d'un Workspace.

**Format Standard** : `/api/v1/workspaces/{workspace_id}/[resource]`

**Exemples** :
- `GET /api/v1/workspaces/ws_92f/companies` -> Liste les entreprises du workspace `ws_92f`.
- `POST /api/v1/workspaces/ws_92f/organizational-units` -> Crée un département dans ce workspace.

### 3. Documentation "Code-First" (SpringDoc OpenAPI)
- **Convention** : Aucun fichier Swagger YAML ne sera écrit à la main.
- Les développeurs Java annotent les contrôleurs Spring avec les annotations `@Operation`, `@ApiResponse` de la librairie SpringDoc.
- SpringDoc expose dynamiquement le fichier OpenAPI 3.0 à l'URL `/v3/api-docs`.

### 4. Code Generation pour le Frontend
- Le frontend React utilise `openapi-generator-cli`.
- Ce script lit l'URL `/v3/api-docs` du backend et génère les interfaces TypeScript et les hooks `React Query` automatiquement.
- **Bénéfice** : Zéro code boilerplate (fetch, axios) écrit à la main côté frontend. Si le backend change un type, le build frontend échouera (Type Safety).

## Décision finale
L'utilisation de la génération TypeScript depuis l'OpenAPI Java permet à l'équipe backend de garder le plein contrôle sur le contrat d'interface, tout en offrant une Developer Experience (DX) exceptionnelle à l'équipe frontend.
