# Phase 1 - Architecture, Design, Planification et Choix Technologique

## Objectif
Définir la fondation complète du SaaS boilerplate : les composants, l'architecture, l'environnement de déploiement, et les technologies. À la fin de cette phase, tu as un blueprint clair pour construire.

---

## Tâches

### 1. Identifier les composants et modules
- [ ] Lister tous les modules réutilisables (auth, payment, dashboard, API, notifications, etc.)
- [ ] Définir les responsabilités de chaque composant
- [ ] Identifier les dépendances entre composants
- [ ] Documenter les data flows

**Livrable :** Composants.md

---

### 2. Choisir l'environnement de déploiement
- [ ] Évaluer les options (Railway, Vercel, AWS, DigitalOcean, Fly.io, etc.)
- [ ] Comparer : coût, scalabilité, facilité, support PostgreSQL/Node
- [ ] Décider pour le backend et le frontend séparément si nécessaire
- [ ] Documenter la justification du choix

**Livrable :** Deployment-Environment.md

---

### 3. Définir l'architecture globale
- [ ] Créer des diagrammes (composants, data flow, deployment)
- [ ] Définir la communication entre services (REST API, webhooks, queues, etc.)
- [ ] Spécifier la structure des dossiers/repos
- [ ] Décider : monorepo vs. multi-repos

**Livrable :** Architecture.md + diagrammes (Mermaid/Excalidraw)

---

### 4. Définir la tech stack complète
- [ ] **Backend** : Runtime, framework, ORM, database, caching, job queue, logging
- [ ] **Frontend** : Framework, build tool, styling, state management, UI library
- [ ] **DevOps** : Hosting, CI/CD, monitoring, error tracking
- [ ] **Paiement & Services externes** : Stripe, email service, etc.
- [ ] Pour chaque choix : expliquer le "pourquoi"

**Livrable :** Tech-Stack.md

---

## Livrables Phase 1

À la fin de cette phase, tu dois avoir :

```
docs/
├── PHASE_1.md                    (ce fichier - vue d'ensemble)
├── 01-composants.md              (modules et responsabilités)
├── 02-deployment-environment.md  (choix d'environnement)
├── 03-architecture.md            (diagrammes + flux)
├── 04-tech-stack.md              (technologies + justifications)
├── 05-database-schema.md         (tables + relations)
├── 06-project-structure.md       (organisation des dossiers)
├── 07-api-design.md              (patterns REST + conventions)
├── 08-decisions.md               (decision log)
└── diagrams/
    ├── architecture-diagram.md   (Mermaid)
    ├── data-flow-diagram.md      (Mermaid)
    └── deployment-diagram.md     (Mermaid)
```

### Checklist finale

- [ ] Architecture Diagram — composants + data flows
- [ ] Tech Stack Document — chaque technologie + justification + versions
- [ ] Database Schema — tables, relations, indexes (générique/réutilisable)
- [ ] Project Structure — comment organiser les fichiers
- [ ] API Design Doc — patterns REST, conventions, exemples
- [ ] Deployment Strategy — environments (dev/staging/prod), process
- [ ] Decision Log — pourquoi tu as choisi ça vs. autre chose

---

## Format de Documentation

### Pour chaque document (ex: Tech-Stack.md)

```markdown
# [Titre du Document]

## Vue d'ensemble
[1-2 lignes résumant le contenu]

## Détails

### [Sous-section 1]
- Point 1
- Point 2
- Justification/rationale

### [Sous-section 2]
- ...

## Tableau comparatif (si applicable)
| Critère | Option A | Option B | Choix |
|---------|----------|----------|-------|
| Coût | ... | ... | ✓ |
| Scalabilité | ... | ... | ✓ |

## Décision finale
[Résumé du choix + pourquoi]

## Prochaines étapes
- [ ] Tâche 1
- [ ] Tâche 2
```

### Pour les diagrammes (Mermaid)

```mermaid
graph TB
    [Ton diagramme ici]
```

---

## Statut Phase 1

| Tâche | Statut | Commentaires |
|-------|--------|--------------|
| Identifier composants | ✅ Terminé | Workspaces, UO, RBAC, Contacts (Clients externes) validés. |
| Choisir environnement déploiement | ✅ Terminé | AWS (ECS Fargate, RDS, S3, SQS). |
| Définir architecture globale | ✅ Terminé | Monolithe Modulaire (Spring Modulith). |
| Définir tech stack | ✅ Terminé | Java 21, Spring Boot 3.3, React 18, PostgreSQL RLS. |
| Valider & documenter | ⏳ En cours | Rédaction des livrables (docs/04-tech-stack.md...). |

---

## Notes

- Tout est documenté en **Markdown** pour la clarté et la portabilité
- Les diagrammes sont en **Mermaid** (visibles directement sur GitHub)
- Chaque décision doit avoir une **justification claire**
- Si tu changes d'avis = update la doc (versionnage Git)

---

## Prochaine étape

Une fois Phase 1 complétée → **Phase 2 : Infrastructure Setup**

