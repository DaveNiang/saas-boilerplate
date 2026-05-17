# 06 - Project Structure (Structure du Code)

## Vue d'ensemble
Ce document définit l'organisation des fichiers pour le dépôt (monorepo logique ou dossiers séparés). Le Backend est structuré selon les Bounded Contexts.

## Détails

### Structure Globale du Dépôt
Nous optons pour une structure claire séparant `frontend`, `backend` et `infra`.

```text
/saas-boilerplate
  /frontend                 # Application React (Vite)
    /src
      /components           # UI partagée (Shadcn)
      /features             # Code métier frontend (ex: contacts, auth)
      /api                  # Client généré par openapi-generator
  /backend                  # Application Spring Boot (Java 21)
    /src/main/java/com/antigravity/saas
      /core                 # Configs globales (Sécurité, Exception Handling)
      /workspaces           # Bounded Context : Workspaces & UO
      /contacts             # Bounded Context : CRM
      /calendar             # Bounded Context : Planning
      /notifications        # Bounded Context : SQS, Email
      /audit                # Bounded Context : JPA Listeners pour logs
  /infra                    # Terraform code
  /docs                     # Documentation (cette arborescence)
```

### Organisation Interne d'un Module (Spring Modulith)
Dans chaque module métier (ex: `/contacts`), nous appliquons l'architecture Hexagonale/Clean Architecture simplifiée :

```text
/contacts
  /api                  # Controllers REST (Exposition externe)
  /internal             # C'est ici que Spring Modulith applique sa magie !
    /domain             # Entités JPA (Person, Company)
    /repository         # Interfaces Spring Data
    /service            # Logique métier pure
  ContactFacade.java    # SEULE classe autorisée à être appelée par les AUTRES modules.
```

## Décision finale
L'organisation du backend par "Feature/Context" (Packages-by-feature) plutôt que par type de fichier (Packages-by-layer) est indispensable pour Spring Modulith. 

## Prochaines étapes
- [ ] Lancer `mvn archetype` ou Spring Initializr pour générer la structure backend.
