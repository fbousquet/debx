# Roadmap Déploiement CRM Pipedrive - Debex-Médical

> **MISE À JOUR 30/12/2024** : Pivot vers architecture CRM simplifié + Module Projets
> Voir `docs/architecture-v2/` pour les nouvelles spécifications

## Vue d'ensemble v2

| Phase | Focus | Objectif | Statut |
|-------|-------|----------|--------|
| **Phase 1** | Architecture | Module Projets + CRM simplifié | 🔄 En cours |
| **Phase 2** | Infrastructure | Serveur, N8N, base de données | 📋 Planifié |
| **Phase 3** | Automatisations | Workflows N8N, alertes | 📋 Planifié |
| **Phase 4** | Déploiement | Formation, optimisation | 📋 Planifié |

## Prochaine étape : Atelier 2 janvier 2025

**Voir** : `docs/roadmap/atelier-2-janvier.md`

Objectifs de l'atelier :
1. Configurer le Module Projets Pipedrive
2. Créer le premier modèle de projet "Centre Hospitalier"
3. Créer 5-6 projets pour les établissements en cours
4. Valider les champs CRM réglementaires

## Changements clés vs v1

| Avant (v1) | Après (v2) |
|------------|------------|
| Pipeline 1 : Qualification Médecin | **Module Projets** : 1 projet par établissement |
| Pipeline 2 : Implémentation | Intégré dans Module Projets |
| Deal = 1 médecin | Projet = 1 établissement |
| Étapes commerciales dans CRM | Phases dans Module Projets |
| CRM complet | **CRM simplifié** (réglementaire uniquement) |

## Jalons clés v2

| Jalon | Description | Date cible |
|-------|-------------|------------|
| M1 | Module Projets configuré | 2 janvier |
| M2 | Premiers projets créés | 2 janvier |
| M3 | Serveur opérationnel | 10 janvier |
| M4 | N8N + automatisations | 15 janvier |
| M5 | Interface soignants | 20 janvier |
| M6 | Équipe formée v2 | Fin janvier |

## Infrastructure technique

| Composant | Fournisseur | Coût | Statut |
|-----------|-------------|------|--------|
| CRM | Pipedrive | 171€/mois | ✅ Actif |
| Module Projets | Pipedrive | Inclus | ✅ Activé |
| Serveur | Hostinger | ~10€/mois | ✅ Commandé |
| Automatisations | N8N (self-hosted) | 0€ | 📋 À installer |
| Base de données | PostgreSQL | 0€ | 📋 À migrer |

## Documentation

- `docs/architecture-v2/overview.md` - Vue d'ensemble nouvelle architecture
- `docs/architecture-v2/crm-reglementaire.md` - Spécifications CRM
- `docs/architecture-v2/module-projets.md` - Spécifications Projets
- `docs/architecture-v2/infrastructure.md` - Serveur et outils
- `docs/roadmap/atelier-2-janvier.md` - Plan atelier configuration

## Contacts projet

| Rôle | Nom | Responsabilité |
|------|-----|----------------|
| DSI | Fabien | Configuration, automatisations, infrastructure |
| Commercial | Yann | Pilotage projets, feedback terrain |
| Commercial | Ben | Opérationnel, réseau médical |
| Commercial | William | Scientifique, suivi médical |

---

## Archive v1 (obsolète)

Les anciennes spécifications sont conservées dans `docs/pipelines/` pour référence mais ne sont plus applicables.
