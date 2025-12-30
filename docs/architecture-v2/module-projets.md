# Module Projets Pipedrive - Spécifications

**Objectif** : Piloter l'implémentation commerciale par établissement hospitalier

---

## Principes directeurs

1. **1 Projet = 1 Établissement** (pas une personne)
2. **Étapes standardisées** : Même parcours pour tous les établissements
3. **Modèles réutilisables** : Templates par typologie de centre
4. **Visibilité équipe** : Suivi collaboratif de l'avancement

---

## Structure hiérarchique

```
GROUPE HOSPITALIER (champ, pas une entité)
│
└── PROJET = ÉTABLISSEMENT
    │
    ├── Phase 1: PROSPECT
    │   ├── Tâche: Identifier décideur
    │   └── Tâche: Premier contact
    │
    ├── Phase 2: LEAD
    │   ├── Tâche: RDV formation théorique
    │   ├── Tâche: Définir protocole douleur
    │   └── Tâche: Demander autorisation multi-centres
    │
    ├── Phase 3: ESSAIS
    │   ├── Tâche: Validation pharmaceutique
    │   ├── Tâche: Commande échantillons Laphal
    │   ├── Tâche: Formation IDE théorique
    │   ├── Tâche: Formation IDE pratique
    │   └── Tâche: Essais patients
    │
    ├── Phase 4: GO/NO-GO
    │   ├── Tâche: Évaluation résultats
    │   └── Tâche: Décision poursuite
    │
    ├── Phase 5: RÉFÉRENCEMENT
    │   ├── Tâche: Négociation conditions
    │   └── Tâche: Première commande officielle
    │
    └── Phase 6: DÉPLOIEMENT
        ├── Sous-projet: Service 1
        ├── Sous-projet: Service 2
        └── Sous-projet: Service N
```

---

## Phases détaillées

### Phase 1 : PROSPECT

**Objectif** : Établir le premier contact avec l'établissement

**Critères d'entrée** :
- Établissement identifié comme cible
- Au moins un contact potentiel

**Tâches types** :
| Tâche | Responsable | Livrable |
|-------|-------------|----------|
| Identifier décideur | Commercial | Contact créé dans CRM |
| Recherche établissement | Commercial | Fiche organisation complète |
| Premier contact | Commercial | Activité CRM enregistrée |

**Critères de sortie** → LEAD :
- Un décideur a manifesté son intérêt

---

### Phase 2 : LEAD

**Objectif** : Obtenir l'accord du médecin pour tester le produit

**Critères d'entrée** :
- Décideur intéressé identifié
- RDV planifiable

**Tâches types** :
| Tâche | Responsable | Livrable |
|-------|-------------|----------|
| Planifier RDV formation | Commercial | RDV confirmé |
| Formation théorique | Directeur Médical | Formation réalisée |
| Définir protocole douleur | Directeur Médical | Protocole validé |
| Autorisation multi-centres | Directeur Médical | Autorisation obtenue (ou refusée) |

**Critères de sortie** → ESSAIS :
- Médecin dit "OK pour tester"
- Protocole douleur défini

---

### Phase 3 : ESSAIS

**Objectif** : Réaliser les essais cliniques et former les équipes

**Critères d'entrée** :
- Accord médecin obtenu
- Pharmacien identifié

**Tâches types** :
| Tâche | Responsable | Livrable |
|-------|-------------|----------|
| Dossier pharmaceutique | Commercial | Dossier envoyé |
| Validation pharma | Pharmacien | Validation obtenue |
| Commande échantillons | Commercial | Email Laphal envoyé |
| Réception échantillons | Commercial | Livraison confirmée |
| Formation IDE théorique | Commercial | IDE formées (théorie) |
| Formation IDE pratique | Commercial | IDE formées (pratique) |
| Identification patients | Médecin | Patients identifiés |
| Réalisation essais | Équipe soignante | Essais terminés |
| Questionnaires post-essai | Commercial | 3 questionnaires envoyés |

**Critères de sortie** → GO/NO-GO :
- Essais terminés
- Résultats disponibles

---

### Phase 4 : GO/NO-GO

**Objectif** : Prendre la décision de poursuivre ou non

**Critères d'entrée** :
- Essais terminés
- Feedback équipes recueilli

**Tâches types** :
| Tâche | Responsable | Livrable |
|-------|-------------|----------|
| Analyse résultats | Commercial | Synthèse |
| Réunion décision | Équipe | Décision documentée |

**Issues possibles** :
| Décision | Action |
|----------|--------|
| GO - Concluant | → Phase RÉFÉRENCEMENT |
| NO-GO - Nouvel essai | → Retour Phase ESSAIS |
| NO-GO - Abandonné | → Projet clôturé (perdu) |

---

### Phase 5 : RÉFÉRENCEMENT

**Objectif** : Obtenir le référencement officiel du produit

**Critères d'entrée** :
- Décision GO confirmée

**Tâches types** :
| Tâche | Responsable | Livrable |
|-------|-------------|----------|
| Négociation conditions | Commercial | Conditions validées |
| Référencement pharmacie | Pharmacien | Produit référencé |
| Première commande | Commercial | Commande passée |

**Critères de sortie** → DÉPLOIEMENT :
- Produit officiellement référencé
- Première commande reçue

---

### Phase 6 : DÉPLOIEMENT

**Objectif** : Étendre l'utilisation à d'autres services

**Critères d'entrée** :
- Référencement obtenu
- Au moins un service utilisateur

**Structure** :
- Création de sous-tâches par service
- Chaque service a son propre mini-cycle : Identification → Formation → Utilisation

**Suivi** :
| Métrique | Description |
|----------|-------------|
| Nb services déployés | Services actifs / Services cibles |
| Volume prescriptions | Commandes par période |
| Satisfaction | Score NPS par service |

---

## Modèles de projets

### Modèle 1 : Centre Prioritaire (CHU)

**Usage** : Grands CHU, volume important
**Spécificités** :
- Toutes les phases détaillées
- Sous-tâches par service dès la phase ESSAIS
- Suivi intensif (revue hebdomadaire)

### Modèle 2 : Centre Standard

**Usage** : CH, cliniques moyennes
**Spécificités** :
- Phases standard
- Moins de sous-tâches
- Suivi bi-mensuel

### Modèle 3 : Petit Centre

**Usage** : EHPAD, petites cliniques
**Spécificités** :
- Phases simplifiées (PROSPECT → LEAD → RÉFÉRENCEMENT)
- Maximum d'automatisation
- Suivi mensuel

---

## Champs personnalisés du projet

### Identification

| Champ | Type | Options |
|-------|------|---------|
| Groupe hospitalier | Dropdown | APHP / APHM / HCL / Autre |
| Établissement | Texte | Nom complet |
| Région | Dropdown | Régions France |
| Code FINESS | Texte | Identifiant |

### Dimensionnement

| Champ | Type | Notes |
|-------|------|-------|
| Nombre de lits | Nombre | Potentiel volume |
| Nb services cibles | Nombre | Services pouvant utiliser |
| CA potentiel | Devise | Estimation annuelle |

### Contacts clés (liens CRM)

| Champ | Type | Notes |
|-------|------|-------|
| Décideur principal | Lien Personne | Le médecin qui a dit oui |
| Pharmacien | Lien Personne | Validateur pharma |
| Cadre de santé | Lien Personne | Organisateur formations |
| IDE référente | Lien Personne | Infirmière principale |
| Opérateurs | Multi-lien | Équipe utilisatrice |

### Suivi

| Champ | Type | Notes |
|-------|------|-------|
| Date début projet | Date | Première activité |
| Protocole douleur | Dropdown | Non / En cours / Oui |
| Autorisation multi-centres | Dropdown | Non demandée / En attente / Accordée / Refusée |
| CA réalisé | Devise | Commandes effectives |

---

## Tableaux de bord Projets

### Vue d'ensemble

| Métrique | Visualisation |
|----------|---------------|
| Projets par phase | Kanban / Graphique barres |
| Progression globale | Entonnoir |
| Projets bloqués | Liste avec durée blocage |

### Par projet

| Métrique | Calcul |
|----------|--------|
| Durée dans phase actuelle | Date actuelle - Date entrée phase |
| Temps total projet | Date actuelle - Date début |
| Tâches complétées | % tâches terminées |
| Prochaine échéance | Tâche avec deadline la plus proche |

### Performance commerciale

| Métrique | Calcul |
|----------|--------|
| Taux de conversion | Projets RÉFÉRENCÉ / Projets démarrés |
| CA par projet | Total CA réalisé / Nb projets |
| Temps moyen référencement | Moyenne durée PROSPECT → RÉFÉRENCEMENT |

---

## Groupement des projets

### Par groupe hospitalier
```
APHP
├── Pitié-Salpêtrière (Phase: ESSAIS)
├── HEGP (Phase: LEAD)
└── Tenon (Phase: PROSPECT)

APHM
├── Conception (Phase: ESSAIS)
└── Timone (Phase: ESSAIS)
```

### Par région
```
Île-de-France
├── APHP - Pitié-Salpêtrière
├── APHP - HEGP
└── ...

Bretagne
├── CHU Brest
└── CHU Rennes
```

---

## Intégration avec le CRM

### Liens automatiques

1. **Activité CRM** → **Tâche projet**
   - Quand une activité est créée avec un contact lié à un projet
   - Optionnel : création automatique d'une tâche dans le projet

2. **Contact CRM** → **Champ projet**
   - Contacts clés référencés dans les champs du projet
   - Historique accessible depuis le projet

### Synchronisation

- Les notes du projet sont visibles dans le CRM
- Les activités CRM apparaissent dans la timeline du projet
- Les mises à jour de champs se propagent (à configurer)
