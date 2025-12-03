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
- 3 commerciaux terrain
- 1 directeur médical (RDV avec médecins référents via Teams)

### Stack technique
- **CRM** : Pipedrive
- **Suite bureautique** : Office 365
- **Documents** : Word (devis, dossiers pharma)
- **Questionnaires** : Fillout
- **Emailing** : Pipedrive natif
- **RDV** : Microsoft Teams (lien envoyé via Pipedrive)

## Architecture CRM

### Pipelines Pipedrive

**Pipeline 1 : Qualification Médecin**
Objectif : Amener le médecin référent à accepter de tester
1. Nouveau Lead
2. Recherche Médecin Référent
3. 1er Contact Médecin
4. RDV Planifié
5. Formation Théorique Réalisée
6. Intéressé pour Tester → Pipeline 2
7. Perdu

**Pipeline 2 : Implémentation & Référencement**
Objectif : Du "oui pour tester" jusqu'à la 1ère commande
1. Validation Pharmaceutique
2. Commande Échantillons
3. Échantillons Livrés
4. Formation IDE Théorique Planifiée
5. Formation IDE Théorique Réalisée
6. Formation IDE Pratique Réalisée
7. Essais en Cours
8. Essais Terminés (→ Concluant / Non concluant - Nouvel essai / Abandonné)
9. Négociation Référencement
10. Gagné - Référencé / Perdu

**Pipeline 3 (optionnel) : Prescription Active**
Suivi post-référencement

### Structure multi-contacts par deal

**Contrainte Pipedrive** : 1 contact principal + participants illimités

**Solution Debex** :
- **Contact principal** = Médecin Référent (décisionnaire unique)
- **Champs Personne** (filtrables dans rapports) :
  - Médecin Référent
  - Pharmacien Principal
  - Cadre de Santé
  - IDE Référente
  - IDE Backup
  - Contact Intermédiaire (qui a donné le contact du médecin)
- **Participants** = Tous les autres (autres médecins, autres IDE, direction...)

## Documentation projet

```
docs/
├── roadmap/
│   ├── overview.md          # Vue macro du projet
│   ├── semaine-1.md         # Fondations
│   ├── semaine-2.md         # Quick wins automatisations
│   ├── semaine-3.md         # Workflow documents
│   └── semaine-4.md         # Continuité & satisfaction
├── pipelines/
│   ├── pipeline-1-qualification.md
│   ├── pipeline-2-implementation.md
│   └── champs-personnalises.md
├── automatisations/
│   ├── index.md             # Liste des 19 automatisations
│   ├── relances.md          # A4, A5, A9, A14
│   ├── workflows.md         # A6, A7, A11
│   ├── documents.md         # A8, A10
│   ├── questionnaires.md    # A12, A13, A19
│   └── data-quality.md      # A1, A2, A3, A16, A17
└── specifications/
    └── client-pieces.md     # Spécifications fonctionnelles futures
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

## Priorités automatisations (TOP 5)
1. A4 - Séquence relance post 1er contact
2. A5 - Alerte deal stagne +7j
3. A17 - Champs obligatoires avant progression
4. A8 - Envoi auto dossier pharma
5. A7 - Création auto deal Pipeline 2

## Conventions de nommage

### Deals
`[Établissement] - [Service] - [Médecin référent]`

### Activités
- `Appel - [Objet]`
- `RDV - [Type] - [Interlocuteur]`
- `Email - [Objet]`

### Tags contacts
- Rôle : Médecin Référent / Pharmacien / Cadre / IDE Référente / IDE Backup
- Spécialité : Néphro / Réa / Autre

## Champs stratégiques ajoutés (suite analyse flowchart)

### Source Lead (granularité accrue)
- Médecin Direct / Médecin Inbound
- Pharmacien Direct / Pharmacien Inbound
- Cadre-IDE Direct / Cadre-IDE Inbound

### Protocole Douleur
- `Protocole Douleur Défini` : Non / En cours / Oui
- `Autorisation Multi-Centres` : Non demandée / En attente / Accordée / Refusée

> L'autorisation multi-centres permet de répliquer le protocole douleur dans d'autres établissements.

### Formation IDE (2 phases)
- `Formation IDE Théorique` : Non planifiée / Planifiée / Réalisée
- `Formation IDE Pratique` : Non planifiée / Planifiée / Réalisée
- `Date Formation IDE` / `Lieu Formation IDE`

### Résultat Essais (avec reboucle)
- `Résultat Essais` : Non évalué / Concluant / Non concluant - Nouvel essai / Non concluant - Abandonné

> Si "Non concluant - Nouvel essai", le deal peut revenir à l'étape "Essais en Cours".
