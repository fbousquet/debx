# Architecture Debex v2 - Vue d'ensemble

**Version** : 2.0
**Date** : 30 décembre 2024
**Statut** : Validé

---

## Contexte du changement

### Problème identifié

L'architecture initiale (v1) basée sur des **pipelines par médecin** ne correspondait pas à la réalité opérationnelle :

- Un établissement implique 10-15 personnes différentes
- Le suivi se fait par établissement, pas par personne
- La qualification d'un médecin ≠ la qualification d'un établissement
- Les étapes commerciales (essais, référencement) sont liées à l'établissement

### Solution adoptée

Séparation claire en deux systèmes complémentaires :

| Système | Objectif | Granularité |
|---------|----------|-------------|
| **CRM** | Conformité réglementaire | Par personne |
| **Projets** | Pilotage commercial | Par établissement |

---

## Architecture cible

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           DEBEX - SYSTÈME INTÉGRÉ                           │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│   ┌─────────────────────┐              ┌─────────────────────┐              │
│   │   CRM PIPEDRIVE     │              │  MODULE PROJETS     │              │
│   │                     │◄────────────►│                     │              │
│   │  • Personnes        │   Liens      │  • Établissements   │              │
│   │  • Activités        │   bidirec-   │  • Phases           │              │
│   │  • Consentements    │   tionnels   │  • Tâches           │              │
│   │  • Qualifications   │              │  • Jalons           │              │
│   └─────────────────────┘              └─────────────────────┘              │
│            │                                      │                         │
│            │                                      │                         │
│            ▼                                      ▼                         │
│   ┌─────────────────────┐              ┌─────────────────────┐              │
│   │  TABLEAUX DE BORD   │              │  TABLEAUX DE BORD   │              │
│   │  CONFORMITÉ         │              │  OPÉRATIONNELS      │              │
│   │                     │              │                     │              │
│   │  • Couverture       │              │  • Avancement       │              │
│   │  • Fréquence        │              │  • CA/établissement │              │
│   │  • Transparence     │              │  • ROI              │              │
│   └─────────────────────┘              └─────────────────────┘              │
│                                                                             │
├─────────────────────────────────────────────────────────────────────────────┤
│                           INFRASTRUCTURE                                    │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│   ┌─────────────────────┐    ┌─────────────────────┐                        │
│   │  SERVEUR HOSTINGER  │    │  BASE DE DONNÉES    │                        │
│   │                     │    │                     │                        │
│   │  • Ubuntu           │    │  • PostgreSQL       │                        │
│   │  • N8N              │    │  • Soignants        │                        │
│   │  • Interface web    │    │  • Import CRM       │                        │
│   └─────────────────────┘    └─────────────────────┘                        │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Composant 1 : CRM Réglementaire

### Objectif
Répondre aux obligations réglementaires de traçabilité des interactions avec les professionnels de santé.

### Ce qu'on y trouve

**Personnes** (contacts)
- Identification : nom, RPPS, établissement, service
- Qualification : utilisateur régulier / décideur / KOL
- Consentements : E32, E34, E35
- Réseau : liens vers autres contacts

**Activités** (interactions)
- Repas (max 2/an/personne)
- Face-face
- Vidéo
- Téléphone
- Congrès
- Webinar

### Ce qu'on n'y trouve plus
- ~~Pipeline de qualification~~
- ~~Étapes commerciales~~
- ~~Suivi d'affaires~~

### Automatisations CRM
| Automatisation | Déclencheur | Action |
|----------------|-------------|--------|
| Alerte repas | 2ème repas/an | Notification |
| Alerte retard visite | Délai dépassé selon qualification | Notification |
| Export transparence | Mensuel | Extraction données repas |

---

## Composant 2 : Module Projets

### Objectif
Piloter l'implémentation commerciale par établissement.

### Structure

```
GROUPE HOSPITALIER (optionnel)
└── ÉTABLISSEMENT (= 1 Projet)
    ├── Phase 1: PROSPECT
    ├── Phase 2: LEAD
    ├── Phase 3: ESSAIS
    ├── Phase 4: GO/NO-GO
    ├── Phase 5: RÉFÉRENCEMENT
    └── Phase 6: DÉPLOIEMENT
        ├── Service 1
        ├── Service 2
        └── Service N
```

### Modèles de projets

| Modèle | Usage | Particularités |
|--------|-------|----------------|
| Centre prioritaire | CHU, gros volume | Suivi intensif, toutes les phases |
| Centre standard | CH, clinique | Phases standard |
| Petit centre | EHPAD, petite clinique | Phases simplifiées, automatisation max |

### Champs par projet
- Groupe hospitalier
- Établissement
- Région
- Nombre de lits
- CA potentiel / CA réel
- Contacts clés (liens CRM)

---

## Liens CRM ↔ Projets

### Principe
Chaque **projet** (établissement) référence des **personnes** (CRM) qui y sont impliquées.

### Mécanisme
1. Une personne est créée dans le CRM
2. Elle est rattachée à un ou plusieurs projets
3. Les activités CRM (visites, appels) alimentent l'historique du projet

### Exemple

```
Projet: CHU Brest
├── Décideur: Dr Langeron (Réanimation)
├── Opérateurs référents:
│   ├── Équipe Plaies (Service Vasculaire)
│   └── IDE Référente
├── Pharmacien: M. Dupont
└── Activités récentes:
    ├── 15/12 - Face-face Dr Langeron
    ├── 20/12 - Vidéo équipe Plaies
    └── 28/12 - Email pharmacien
```

---

## Infrastructure technique

### Serveur Hostinger
- **OS** : Ubuntu (dernière version)
- **Coût** : ~10€/mois
- **Usage** : Hébergement N8N + base de données + interface

### N8N (Automatisations)
- **Coût** : Gratuit (self-hosted)
- **Usage** : Automatisations avancées non possibles dans Pipedrive

### Base de données PostgreSQL
- **Migration** : Depuis Supabase (cloud)
- **Contenu** : Base complète des soignants français
- **Interface** : Application web pour recherche et import CRM

---

## Coûts mensuels

| Service | Avant | Après | Économie |
|---------|-------|-------|----------|
| Pipedrive | 147€ | 171€* | -24€ |
| Microsoft 365 | 185€ | ~155€** | +30€ |
| Supabase | ~25€ | 0€ | +25€ |
| Hostinger | 0€ | 10€ | -10€ |
| **TOTAL** | ~357€ | ~336€ | **+21€** |

*Ajout module Projets + utilisateur William
**Optimisation abonnements Microsoft (à confirmer)

---

## Prochaines étapes

### Semaine 1 (30 déc - 5 jan)
- [x] Serveur Hostinger commandé
- [x] Module Projets activé
- [ ] Atelier configuration (2 janvier)
- [ ] Premiers projets créés

### Semaine 2 (6-12 jan)
- [ ] Installation N8N
- [ ] Migration base de données
- [ ] Interface recherche soignants
- [ ] Formation équipe

### Semaine 3+ (13 jan+)
- [ ] Automatisations N8N
- [ ] Tableaux de bord
- [ ] Optimisation continue
