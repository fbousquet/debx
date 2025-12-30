# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Contexte Projet

**Client** : Debex-Médical.fr
**Mission** : DSI as a Service - Déploiement CRM Pipedrive + automatisations
**Produit** : Dispositif médical d'aide à la cicatrisation (Debrichem)
**Cible** : Professionnels de santé B2B (hôpitaux)

### Segments intégrés dans le CRM
- Néphrologie (nephro)
- Réanimation (reanimation)

### Équipe
- 3 commerciaux terrain (Yann, Ben, William)
- 1 directeur médical (RDV avec médecins référents via Teams)

### Stack technique
- **CRM** : Pipedrive (CRM réglementaire + Module Projets)
- **Suite bureautique** : Office 365 (optimisation en cours)
- **Documents** : Word (devis, dossiers pharma)
- **Questionnaires** : Fillout
- **Emailing** : Pipedrive natif
- **RDV** : Microsoft Teams (lien envoyé via Pipedrive)
- **Serveur** : Hostinger VPS (~10€/mois) - Ubuntu
- **Automatisations** : N8N (self-hosted, gratuit)
- **Base de données** : PostgreSQL sur serveur Hostinger (migration depuis Supabase)

## Architecture CRM (v2 - Décision 30/12/2024)

> **PIVOT MAJEUR** : Séparation claire entre CRM réglementaire (suivi des contacts) et Gestion de Projet (suivi des établissements)

### Principe fondamental

```
┌─────────────────────────────────────────────────────────────────┐
│                    ARCHITECTURE DEBEX v2                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   CRM PIPEDRIVE                    MODULE PROJETS PIPEDRIVE     │
│   (Suivi réglementaire)            (Gestion par établissement)  │
│                                                                 │
│   • Qui on a vu                    • 1 projet = 1 établissement │
│   • Quand (date + matin/PM)        • Étapes standardisées       │
│   • Comment (repas, face-face...)  • Modèles par typologie      │
│   • Conformité E32/E34/E35         • Suivi temporel             │
│   • Qualifications contacts        • Affectation ressources     │
│                                                                 │
│         ↓                                    ↓                  │
│   OBLIGATION RÉGLEMENTAIRE         PILOTAGE OPÉRATIONNEL        │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 1. CRM Réglementaire (Suivi des Personnes)

**Objectif** : Conformité réglementaire - tracer tous les contacts avec les professionnels de santé

**Données obligatoires par contact** :
- Qui : Nom, prénom, établissement, service, RPPS
- Quand : Date + matin/après-midi
- Comment : Type de relation (voir ci-dessous)
- Consentements : E32 (contact), E34 (établissement), E35

**Types de relations (activités)** :
- Repas (max 2/an/personne - alerte automatique)
- Face-face (présentiel)
- Vidéo (Teams)
- Téléphone
- Congrès
- Webinar

**Qualifications des contacts** :
| Qualification | Fréquence de visite |
|--------------|---------------------|
| Utilisateur régulier | 1x / mois |
| Décideur | 1x / trimestre |
| KOL | 1x / 6 mois |

> La fréquence la plus soutenue l'emporte (ex: utilisateur régulier + décideur = 1x/mois)

**Champs additionnels sur les Personnes** :
- Réseau (liens vers autres médecins)
- Société savante
- Études cliniques (fichiers)
- Étiologie/spécialité

### 2. Module Projets (Suivi des Établissements)

**Objectif** : Piloter l'implémentation du produit par établissement

**Structure** : 1 Projet = 1 Établissement (pas une personne)

**Étapes du projet** :
```
PROSPECT → LEAD → ESSAIS → GO/NO-GO → RÉFÉRENCEMENT → DÉPLOIEMENT
    │        │       │         │            │              │
    │        │       │         │            │              └─ Par service (S1, S2...)
    │        │       │         │            └─ Pharmacie valide
    │        │       │         └─ Décision essais
    │        │       └─ Tests produit
    │        └─ Intérêt confirmé
    └─ Contact initial
```

**Modèles de projets** (à créer) :
- Centre prioritaire (CHU, gros volume)
- Centre standard (CH, clinique)
- Petit centre (EHPAD, clinique petite)

**Groupement possible** :
- Par groupe hospitalier (APHP, APHM, HCL...)
- Par GHT
- Par région

### 3. Liens CRM ↔ Projets

- Chaque contact CRM peut être lié à un projet
- Une activité dans le CRM alimente automatiquement le projet correspondant
- Le projet référence tous les contacts impliqués (décideurs, opérateurs, pharmaciens...)

### Ancienne architecture (obsolète)

~~**Pipeline 1 : Qualification Médecin**~~ → Remplacé par Module Projets
~~**Pipeline 2 : Implémentation & Référencement**~~ → Intégré dans Module Projets
**Pipeline 3 : Prescription Active** → À évaluer ultérieurement

## Documentation projet

```
docs/
├── architecture-v2/
│   ├── overview.md              # Nouvelle architecture CRM + Projets
│   ├── crm-reglementaire.md     # CRM simplifié (conformité)
│   ├── module-projets.md        # Gestion de projet par établissement
│   ├── tableaux-de-bord.md      # KPIs et dashboards
│   └── infrastructure.md        # Serveur, N8N, base de données
├── roadmap/
│   ├── overview.md              # Vue macro du projet (mise à jour)
│   ├── atelier-2-janvier.md     # Plan atelier configuration Projets
│   └── archive/                 # Anciennes semaines (obsolètes)
├── pipelines/                   # OBSOLÈTE - voir architecture-v2
│   └── (fichiers archivés)
├── automatisations/
│   ├── index.md                 # Liste des automatisations (à réviser)
│   └── ...
└── specifications/
    └── client-pieces.md
```

## Cycle de vente Debex

```
Phase 0: Prospection (multi-canaux)
    - Via Médecins (Direct / Inbound)
    - Via Pharmaciens (Direct / Inbound) → réorientation médecin
    - Via Cadres/IDE (Direct / Inbound) → réorientation médecin
    ↓
Phase 1: Convaincre médecin de tester
    - 1er contact → Formation théorique → Intérêt
    - Définition protocole douleur + autorisation multi-centres
    ↓
Phase 2: Validation & Préparation
    - Dossier pharmaceutique → Commande échantillons Laphal
    - Formation IDE théorique (protocoles)
    - Formation IDE pratique (avec produit)
    ↓
Phase 3: Essais patients
    - Patients identifiés → Essais → Questionnaires satisfaction (3 destinataires)
    - Si non concluant → possibilité nouvel essai
    ↓
Phase 4: Référencement
    - Négociation → 1ère commande
    ↓
Phase 5: Prescription active (Pipeline 3)
    - Faire prescrire (Médecins, IDE, Pharmaciens)
```

## Fournisseur échantillons
**Laphal** : Commande échantillons via email pré-formaté

## Priorités (mise à jour 30/12/2024)

### Immédiat (Atelier 2 janvier)
1. **Configuration Module Projets** - Créer le premier modèle de projet
2. **Simplification CRM** - Réduire aux champs réglementaires obligatoires
3. **Lien CRM ↔ Projets** - Comprendre l'intégration native Pipedrive

### Infrastructure (Fabien)
1. Installation serveur Hostinger (Ubuntu)
2. Déploiement N8N
3. Migration base de données depuis Supabase
4. Interface recherche soignants

### Automatisations (à réviser selon nouvelle architecture)
- Alerte repas > 2/an/personne (réglementaire)
- Alerte contact non vu selon fréquence qualification
- Extraction transparence (déclarations repas)

## Tableaux de bord requis

### CRM (conformité)
| Métrique | Description |
|----------|-------------|
| Contacts par commercial | Nombre de personnes vues par période |
| Couverture cibles | % de contacts vus dans les délais (selon qualification) |
| Alerte retard | Contacts en retard de visite |

### Projets (opérationnel)
| Métrique | Description |
|----------|-------------|
| Temps passé par établissement | Durée cumulée des activités |
| CA par établissement | Chiffre d'affaires réel |
| Avancement projets | Position dans le cycle par établissement |
| ROI | CA / temps investi |

## Conventions de nommage

### Projets
`[Groupe] - [Établissement]` ou `[Établissement]` si pas de groupe

Exemples :
- `APHP - Pitié-Salpêtrière`
- `APHM - Conception`
- `CHU Brest`

### Activités (CRM)
- `Repas - [Interlocuteur] - [Lieu]`
- `Face-face - [Interlocuteur]`
- `Vidéo - [Interlocuteur]`
- `Congrès - [Nom congrès]`
- `Webinar - [Sujet]`

### Tags contacts
- Qualification : Utilisateur régulier / Décideur / KOL
- Rôle : Médecin / Pharmacien / Cadre / IDE
- Spécialité : Néphro / Réa / Autre

## Champs sur les Personnes (CRM)

### Identification
- Nom, Prénom
- RPPS
- Établissement (lien Organisation)
- Service
- Spécialité

### Qualification
- Type : Utilisateur régulier / Décideur / KOL
- Fréquence visite : Auto-calculée selon type

### Réglementaire
- E32 : Consentement contact (Oui/Non/Date)
- E34 : Politique établissement (Oui/Non)
- E35 : Autre

### Réseau
- Réseau : Liens vers autres Personnes
- Société savante : Texte libre
- Études cliniques : Fichiers attachés

## Champs sur les Projets (Module Projets)

### Identification
- Groupe hospitalier (APHP, APHM, HCL, etc.)
- Établissement
- Région

### Suivi
- Étape actuelle : Prospect / Lead / Essais / Go-NoGo / Référencement / Déploiement
- Date début
- Services concernés (S1, S2...)

### Contacts clés (liens Personnes)
- Décideur principal
- Opérateurs référents
- Pharmacien
- IDE référente

### Protocole
- Protocole Douleur Défini : Non / En cours / Oui
- Autorisation Multi-Centres : Non demandée / En attente / Accordée / Refusée

> L'autorisation multi-centres permet de répliquer le protocole douleur dans d'autres établissements.
