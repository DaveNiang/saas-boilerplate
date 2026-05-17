# Data Flow: Création d'une Entité (Contact) & Audit Asynchrone

Ce diagramme de séquence montre l'application de la sécurité RLS et l'utilisation du bus d'événements pour l'audit sans bloquer l'utilisateur.

```mermaid
sequenceDiagram
    participant UI as React Frontend
    participant API as Spring REST Controller
    participant Security as Spring Security / RBAC
    participant OU_Service as OU & Entity Module
    participant DB as PostgreSQL (RDS)
    participant EventBus as Spring Application Events
    participant Audit_Service as Audit Module

    UI->>API: POST /api/v1/workspaces/{w_id}/contacts
    Note right of UI: JWT Token included
    
    API->>Security: Intercept Request
    Security->>Security: Validate JWT
    Security->>DB: Check Roles for user in {w_id}
    DB-->>Security: User is "Manager"
    Security->>Security: Assert "CREATE_CONTACT" permission
    Security-->>API: Authorized
    
    API->>OU_Service: createContact(DTO)
    OU_Service->>DB: INSERT INTO contacts (workspace_id, ...)
    Note right of OU_Service: RLS active on workspace_id
    DB-->>OU_Service: Success (Contact ID)
    
    OU_Service--)EventBus: Publish ContactCreatedEvent (Async)
    OU_Service-->>API: Contact DTO
    API-->>UI: 201 Created
    
    %% Asynchronous Processing
    EventBus-->>Audit_Service: Consume Event
    Audit_Service->>DB: INSERT INTO audit_logs
```
