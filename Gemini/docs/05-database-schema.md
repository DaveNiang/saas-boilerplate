# 05 - Database Schema (Modèle de Données)

## Vue d'ensemble
Ce document décrit le schéma relationnel initial de l'application. La sécurité multi-tenant est la clé de voûte de ce modèle, reposant sur le principe de Row Level Security (RLS) garanti par PostgreSQL.

## Détails et Conventions

### 1. Row Level Security (RLS)
- **Convention** : TOUTES les tables métier (sans exception) possèderont une colonne `workspace_id`.
- **Pourquoi ?** : Le RLS de PostgreSQL utilisera cet identifiant plat pour bloquer silencieusement toute lecture/écriture d'un locataire sur les données d'un autre. Si un développeur oublie une clause `WHERE workspace_id = ?`, la base de données interceptera l'erreur, garantissant zéro fuite de données.

### 2. Typage des Clés Primaires
- Utilisation systématique de `UUID` (générés côté application ou via `gen_random_uuid()` en BD) pour rendre les ressources non prédictibles (meilleure sécurité que les entiers auto-incrémentés).

## Schéma ERD (Mermaid)

```mermaid
erDiagram
    %% MODULE WORKSPACES & UO
    WORKSPACES {
        uuid id PK
        string name
        datetime created_at
    }
    ORGANIZATIONAL_UNITS {
        uuid id PK
        uuid workspace_id FK
        uuid parent_uo_id FK "Lien récursif"
        string name
    }
    
    %% MODULE AUTH & RBAC
    USERS {
        uuid id PK
        string email
        string keycloak_id "ID externe SSO"
    }
    ROLES {
        uuid id PK
        string name "Admin, Manager, Viewer"
        jsonb permissions
    }
    WORKSPACE_USERS {
        uuid workspace_id FK
        uuid user_id FK
        uuid role_id FK
        uuid uo_id FK "Affectation à une UO"
    }

    %% MODULE CONTACTS (Entités externes)
    COMPANIES {
        uuid id PK
        uuid workspace_id FK
        string name
        string tax_id "SIRET/TVA"
        string address
    }
    CONTACTS {
        uuid id PK
        uuid workspace_id FK
        uuid company_id FK "Optionnel"
        string first_name
        string last_name
        string email
        string phone
    }

    %% MODULE CALENDAR
    EVENTS {
        uuid id PK
        uuid workspace_id FK
        string title
        datetime start_time
        datetime end_time
    }
    EVENT_ATTENDEES {
        uuid event_id FK
        uuid user_id FK "Si interne"
        uuid contact_id FK "Si externe"
    }

    %% RELATIONS
    WORKSPACES ||--o{ ORGANIZATIONAL_UNITS : "possède"
    ORGANIZATIONAL_UNITS ||--o{ ORGANIZATIONAL_UNITS : "sous-département"
    WORKSPACES ||--o{ WORKSPACE_USERS : "a des"
    USERS ||--o{ WORKSPACE_USERS : "membre de"
    ROLES ||--o{ WORKSPACE_USERS : "définit droits"
    ORGANIZATIONAL_UNITS ||--o{ WORKSPACE_USERS : "inclut"
    
    WORKSPACES ||--o{ COMPANIES : "gère clients"
    WORKSPACES ||--o{ CONTACTS : "gère"
    COMPANIES ||--o{ CONTACTS : "emploie"
    
    WORKSPACES ||--o{ EVENTS : "contient"
    EVENTS ||--o{ EVENT_ATTENDEES : "invite"
```

## Décision finale
L'intégration de la colonne `workspace_id` partout permet une scalabilité immense et une sécurité infranchissable. La modélisation de l'arbre `ORGANIZATIONAL_UNITS` par auto-jointure (`parent_uo_id`) est standardisée.

## Prochaines étapes
- [ ] Préparer les scripts Flyway pour ce schéma.
