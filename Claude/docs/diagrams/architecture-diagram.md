# Diagramme des Composants (Haut Niveau)

Ce diagramme représente la vue logique (Level 1 / Level 2 du modèle C4). Il se concentre purement sur les Bounded Contexts (les modules que nous avons validés) et comment ils interagissent entre eux, sans se soucier des serveurs, des bases de données ou de l'infrastructure AWS.

```mermaid

graph TD
    %% Acteurs
    User((Employé du Client))
    Ext[Identity Provider - Keycloak]

    %% SaaS Backend
    subgraph Backend["Backend SaaS (Monolithe Modulaire)"]

        subgraph Technique["Composants Techniques (Niveau 0)"]
            Auth[Identity Adapter]
            WS[Workspace & Tenant Management]
            RBAC[Role-Based Access Control]
            Audit[Audit Trail & Logging]
        end

        subgraph Metier["Composants Métiers (Niveau 1)"]
            OU[OU & Entity Management]
            Comm[Communication & Notifications]
        end

    end

    %% Flux Utilisateur
    User -.->|1. Login OAuth2| Ext
    User -->|2. Requêtes API avec Token| Auth

    %% Flux Synchrones
    Auth -->|Valide le Token| Ext
    RBAC -->|Vérifie le contexte| WS

    OU -->|Requiert autorisation| RBAC
    OU -->|Restreint les données par| WS

    Comm -->|Récupère les préférences de| WS

    %% Flux Asynchrones
    OU -.->|Publie événement ex: Entité Créée| Comm

    OU -.->|Trace mutation| Audit
    WS -.->|Trace mutation| Audit
    RBAC -.->|Trace mutation| Audit
    
```

## Explication des Interactions
- **Flux Synchrone (Lignes pleines)** : C'est un appel direct dans le code (ex: le module OU doit absolument interroger le RBAC et le Workspace pour savoir s'il a le droit de retourner une information).
- **Flux Asynchrone (Pointillés)** : C'est du "Fire and Forget" via le bus d'événements. Par exemple, quand une Unité Organisationnelle est modifiée, le module OU envoie un événement "OU_MODIFIED". L'Audit l'attrape pour l'archiver, sans ralentir l'utilisateur.
