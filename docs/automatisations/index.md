# Index des Automatisations Pipedrive

## Vue d'ensemble

| ID | Nom | Catégorie | Impact | Complexité | Semaine |
|----|-----|-----------|--------|------------|---------|
| A1 | Création auto Organisation | Data Quality | Moyen | Faible | - |
| A2 | Liaison contacts établissement | Data Quality | Moyen | Moyen | - |
| A3 | Assignation par territoire | Distribution | Élevé | Faible | S2 |
| A4 | Séquence relance 1er contact | Relance | Élevé | Faible | S2 |
| A5 | Alerte stagnation 7j | Pipeline Hygiene | Élevé | Faible | S2 |
| A6 | Notification "Intéressé tester" | Workflow | Moyen | Faible | S4 |
| A7 | Création deal Pipeline 2 | Workflow | Élevé | Moyen | S4 |
| A8 | Envoi dossier pharma | Documents | Élevé | Faible | S3 |
| A9 | Alerte validation pharma >10j | Pipeline Hygiene | Moyen | Faible | S3 |
| A10 | Mail commande Laphal | Documents | Élevé | Moyen | S3 |
| A11 | Activité formation IDE | Workflow | Moyen | Faible | S4 |
| A12 | Questionnaire post-formation | Satisfaction | Moyen | Faible | S4 |
| A13 | Questionnaire post-essai | Satisfaction | Moyen | Moyen | S4 |
| A14 | Alerte 14j sans activité | Pipeline Hygiene | Élevé | Faible | S2 |
| A15 | Rapport hebdo deals | Reporting | Moyen | Moyen | Buffer |
| A16 | Détection doublons | Data Quality | Moyen | Faible | Buffer |
| A17 | Champs obligatoires | Data Quality | Élevé | Faible | S1 |
| A18 | Notification échec automation | Monitoring | Moyen | Moyen | Buffer |
| A19 | Webhook Fillout → Pipedrive | Satisfaction | Moyen | Moyen | Buffer |

---

## Par catégorie

### Data Quality
- **A1** - Création auto Organisation depuis Personne
- **A2** - Liaison auto contacts d'un même établissement
- **A16** - Détection et alerte doublons
- **A17** - Champs obligatoires avant progression ⭐ Priorité

### Distribution
- **A3** - Assignation auto par territoire ⭐ Priorité

### Relance
- **A4** - Séquence relance post 1er contact ⭐ Priorité

### Pipeline Hygiene
- **A5** - Alerte deal stagne +7j ⭐ Priorité
- **A9** - Alerte validation pharma >10j
- **A14** - Alerte deal sans activité >14j ⭐ Priorité

### Workflow
- **A6** - Notification médecin "Intéressé pour tester"
- **A7** - Création auto deal Pipeline 2 ⭐ Priorité
- **A11** - Création activité formation IDE

### Documents
- **A8** - Envoi auto dossier pharmaceutique ⭐ Priorité
- **A10** - Envoi auto mail commande Laphal

### Satisfaction
- **A12** - Questionnaire Fillout post-formation
- **A13** - Questionnaire Fillout post-essai
- **A19** - Webhook Fillout → mise à jour Pipedrive

### Reporting
- **A15** - Rapport hebdo deals par étape

### Monitoring
- **A18** - Notification d'échec d'automation

---

## TOP 5 Priorités

| Rang | ID | Nom | Pourquoi |
|------|----|----|----------|
| 1 | A4 | Relance 1er contact | Quick win, récupère des deals perdus |
| 2 | A5 | Alerte 7j | Pipeline propre dès le départ |
| 3 | A17 | Champs obligatoires | Fondation pour toutes les autres |
| 4 | A8 | Envoi dossier pharma | Accélère le cycle, zéro oubli |
| 5 | A7 | Création deal P2 | Continuité parfaite qualification → implémentation |

---

## Dépendances

```
A17 (Champs obligatoires)
    │
    └──► Toutes les automatisations dépendent de données fiables

A7 (Création deal P2)
    │
    └──► A8, A9, A10, A11, A12, A13 (toutes les automatisations P2)

A8 (Dossier pharma)
    │
    └──► A9 (Alerte si pas de validation)
    └──► A10 (Commande si validation OK)

A10 (Commande Laphal)
    │
    └──► A11 (Formation IDE après livraison)

A11 (Activité formation)
    │
    └──► A12 (Questionnaire post-formation)

A12/A13 (Questionnaires)
    │
    └──► A19 (Webhook résultats)
```

---

## Outils requis

| Automatisation | Outil |
|----------------|-------|
| A1-A11, A14, A17 | Pipedrive Workflow Automation (natif) |
| A12, A13 | Pipedrive + Fillout |
| A15 | Pipedrive Insights |
| A19 | Zapier ou Make |

---

## Statut déploiement

| ID | Statut | Date déploiement | Notes |
|----|--------|------------------|-------|
| A1 | À planifier | - | - |
| A2 | À planifier | - | - |
| A3 | À déployer S2 | - | Mapping territoires requis |
| A4 | À déployer S2 | - | Templates email à créer |
| A5 | À déployer S2 | - | - |
| A6 | À déployer S4 | - | - |
| A7 | À déployer S4 | - | - |
| A8 | À déployer S3 | - | PDF dossier requis |
| A9 | À déployer S3 | - | - |
| A10 | À déployer S3 | - | Email Laphal à confirmer |
| A11 | À déployer S4 | - | - |
| A12 | À déployer S4 | - | Questionnaire Fillout requis |
| A13 | À déployer S4 | - | Questionnaire Fillout requis |
| A14 | À déployer S2 | - | - |
| A15 | Buffer | - | - |
| A16 | Buffer | - | - |
| A17 | À déployer S1 | - | Première priorité |
| A18 | Buffer | - | - |
| A19 | Buffer | - | Nécessite Zapier/Make |

---

## Documentation détaillée

- [Automatisations de relance (A4, A5, A9, A14)](./relances.md)
- [Workflows (A6, A7, A11)](./workflows.md)
- [Documents (A8, A10)](./documents.md)
- [Questionnaires (A12, A13, A19)](./questionnaires.md)
- [Data Quality (A1, A2, A3, A16, A17)](./data-quality.md)
