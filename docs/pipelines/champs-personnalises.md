# Champs Personnalisés Pipedrive

## Champs sur les Deals

### Champs de qualification

| Champ | Type | Options | Obligatoire | Pipeline |
|-------|------|---------|-------------|----------|
| Source Lead | Dropdown | Médecin Direct / Médecin Inbound / Pharmacien Direct / Pharmacien Inbound / Cadre-IDE Direct / Cadre-IDE Inbound | Oui (création) | P1 + P2 |
| Spécialité Service | Dropdown | Néphro / Réa / Autre | Oui | P1 + P2 |
| Contact Intermédiaire | Lien Personne | - | Non | P1 |

> **Note** : Le champ "Contact Intermédiaire" permet de tracer qui a donné le contact du médecin référent (pharmacien, cadre ou IDE ayant fait la mise en relation).

### Champs de suivi Pipeline 1

| Champ | Type | Options | Obligatoire | Notes |
|-------|------|---------|-------------|-------|
| Statut Formation Théorique | Dropdown | Non planifiée / Planifiée / Réalisée | - | Mis à jour manuellement |
| Lien RDV Directeur Médical | Texte (URL) | - | Avant "Formation réalisée" | Lien Teams/Calendly |
| Protocole Douleur Défini | Dropdown | Non / En cours / Oui | - | Défini lors du RDV Directeur Médical |
| Autorisation Multi-Centres | Dropdown | Non demandée / En attente / Accordée / Refusée | - | Autorisation d'utiliser le protocole douleur dans d'autres centres |

> **Note Protocole Douleur** : Le protocole douleur est un élément stratégique défini lors du RDV entre le Directeur Médical Debex et le médecin référent. L'obtention de l'autorisation multi-centres permet de répliquer ce protocole dans d'autres établissements.

### Champs de suivi Pipeline 2

| Champ | Type | Options | Obligatoire | Notes |
|-------|------|---------|-------------|-------|
| Statut Validation Pharma | Dropdown | En attente / Validé / Refusé | - | Déclenche A10 si "Validé" |
| Date Commande Échantillons | Date | - | - | Rempli auto par A10 |
| Statut Échantillons | Dropdown | Commandés / Expédiés / Livrés | - | Déclenche A11 si "Livrés" |
| Formation IDE Théorique | Dropdown | Non planifiée / Planifiée / Réalisée | - | Formation théorique (protocole, indications) |
| Formation IDE Pratique | Dropdown | Non planifiée / Planifiée / Réalisée | - | Formation pratique avec produit. Déclenche A12 si "Réalisée" |
| Date Formation IDE | Date | - | - | Date prévue ou réalisée |
| Lieu Formation IDE | Texte | - | - | Salle ou service de formation |
| Statut Essais | Dropdown | Non démarrés / En cours / Terminés | - | Déclenche A13 si "Terminés" |
| Résultat Essais | Dropdown | Non évalué / Concluant / Non concluant - Nouvel essai / Non concluant - Abandonné | - | Permet de relancer un nouvel essai si non concluant |
| Questionnaire Satisfaction Envoyé | Dropdown | Oui / Non | - | Mis à jour auto |

> **Note Formation IDE** : Le PDF commercial distingue clairement 2 phases de formation IDE :
> 1. **Théorique** : Protocole Debrichem, protocole douleur, identification des patients
> 2. **Pratique** : Formation avec le produit, cas réels
>
> **Note Résultat Essais** : Si "Non concluant - Nouvel essai", le deal peut revenir à l'étape "Essais en cours" pour une nouvelle tentative.

### Champs de liaison

| Champ | Type | Options | Notes |
|-------|------|---------|-------|
| Deal P1 Origine | Lien deal | - | Sur deals P2, référence au deal P1 converti |

### Champs de liaison multi-contacts (type Personne)

| Champ | Type | Usage | Obligatoire |
|-------|------|-------|-------------|
| Médecin Référent | Lien Personne | Le décisionnaire unique | Oui (P1 étape 3+) |
| Pharmacien Principal | Lien Personne | Le validateur pharma | Oui (P2 étape 1+) |
| Cadre de Santé | Lien Personne | L'organisateur formations | Recommandé (P2) |
| IDE Référente | Lien Personne | L'infirmière principale formée | Recommandé (P2) |
| IDE Backup | Lien Personne | L'infirmière backup | Optionnel |

---

## Structure multi-contacts des Deals

### Contrainte Pipedrive

Pipedrive impose une limitation :
- **1 seul contact principal** par deal (visible dans les rapports et listes)
- **Participants illimités** (visibles uniquement dans le détail du deal)

### Solution Debex

```
DEAL : CHU Lyon - Néphrologie - Dr Dupont
│
├── CONTACT PRINCIPAL (1 seul, dans les rapports)
│   └── Dr Dupont (Médecin Référent)
│
├── CHAMPS PERSONNALISÉS "Personne" (1 par rôle clé, filtrables)
│   ├── Médecin Référent : Dr Dupont
│   ├── Pharmacien Principal : Mme Martin
│   ├── Cadre de Santé : Mme Durand
│   ├── IDE Référente : Mme Bernard
│   └── IDE Backup : Mme Petit
│
└── PARTICIPANTS (illimités, pour tous les autres)
    ├── Dr Lambert (autre médecin du service)
    ├── Mme Garcia (autre pharmacien)
    ├── Mme Rousseau (autre IDE formée)
    └── ... (direction, autres contacts)
```

### Règles de gestion

| Type de liaison | Qui ? | Quantité | Visible rapports ? |
|-----------------|-------|----------|-------------------|
| Contact principal | Médecin Référent | 1 seul | ✅ Oui |
| Champs Personne | Rôles clés (Pharma, Cadre, IDE) | 1 par champ | ✅ Oui (filtrable) |
| Participants | Tous les autres contacts | Illimité | ❌ Non |

### Qui mettre où ?

| Contact | Contact principal ? | Champ Personne ? | Participant ? |
|---------|---------------------|------------------|---------------|
| Médecin Référent (décisionnaire) | ✅ OUI | ✅ OUI | - |
| Autres médecins du service | - | - | ✅ OUI |
| Pharmacien validateur | - | ✅ OUI | - |
| Autres pharmaciens | - | - | ✅ OUI |
| Cadre de Santé | - | ✅ OUI | - |
| IDE Référente | - | ✅ OUI | - |
| IDE Backup | - | ✅ OUI | - |
| Autres IDE formées | - | - | ✅ OUI |
| Direction (négociation) | - | - | ✅ OUI |

---

## Champs sur les Personnes

### Identification

| Champ | Type | Options | Obligatoire | Notes |
|-------|------|---------|-------------|-------|
| Rôle | Dropdown | Médecin Référent / Pharmacien / Cadre de Santé / IDE Référente / IDE Backup | Recommandé | Utilisé par automatisations |
| Spécialité | Texte | - | - | Libre (ex: Néphrologue, Réanimateur) |

### Préférences contact

| Champ | Type | Options | Notes |
|-------|------|---------|-------|
| Reçoit les VM | Dropdown | Oui / Non | Pour les appels commerciaux |
| Horaires particuliers | Texte | - | Ex: "Joignable mardi AM uniquement" |

---

## Champs sur les Organisations

### Identification

| Champ | Type | Options | Notes |
|-------|------|---------|-------|
| Type établissement | Dropdown | CHU / CH / Clinique / EHPAD / Autre | Segmentation |
| Région | Dropdown | Selon territoires | Utilisé pour assignation auto (A3) |

### Informations complémentaires

| Champ | Type | Options | Notes |
|-------|------|---------|-------|
| Code établissement | Texte | - | Identifiant FINESS ou autre |
| Nombre de lits | Nombre | - | Potentiel de l'établissement |

---

## Configuration des valeurs Dropdown

### Source Lead
```
- Médecin Direct       → Contact direct avec un médecin (prospection terrain)
- Médecin Inbound      → Médecin entrant (site web, salon, recommandation)
- Pharmacien Direct    → Contact direct avec pharmacien hospitalier
- Pharmacien Inbound   → Pharmacien entrant (demande info, salon)
- Cadre-IDE Direct     → Contact direct avec cadre de santé ou IDE
- Cadre-IDE Inbound    → Cadre/IDE entrant (demande info, recommandation)
```

> Cette granularité permet d'analyser les performances par canal d'acquisition ET par type de contact initial.

### Spécialité Service
```
- Néphro            → Service de néphrologie
- Réa               → Service de réanimation
- Autre             → Autres services (préciser en note)
```

### Rôle (Personnes)
```
- Médecin Référent  → Prescripteur principal
- Pharmacien        → Pharmacien hospitalier validateur
- Cadre de Santé    → Responsable équipe soignante
- IDE Référente     → Infirmière principale formée
- IDE Backup        → Infirmière de backup formée
```

### Type établissement
```
- CHU               → Centre Hospitalier Universitaire
- CH                → Centre Hospitalier
- Clinique          → Clinique privée
- EHPAD             → Établissement pour personnes âgées
- Autre             → Autre type de structure
```

### Région (à personnaliser)
```
- Île-de-France
- Grand-Est
- Auvergne-Rhône-Alpes
- Provence-Alpes-Côte d'Azur
- Occitanie
- Nouvelle-Aquitaine
- Bretagne
- Normandie
- Hauts-de-France
- Centre-Val de Loire
- Pays de la Loire
- Bourgogne-Franche-Comté
- DOM-TOM
```

---

## Règles de champs obligatoires (A17)

### Pipeline 1 - Qualification

| Transition vers | Champs requis |
|-----------------|---------------|
| 1er Contact Médecin | Source Lead |
| RDV Planifié | Champ "Médecin Référent" renseigné |
| Formation Théorique Réalisée | Lien RDV Directeur Médical |
| Intéressé pour Tester | Statut Formation Théorique = "Réalisée" |

### Pipeline 2 - Implémentation

| Transition vers | Champs requis |
|-----------------|---------------|
| Validation Pharmaceutique | Champ "Pharmacien Principal" renseigné |
| Commande Échantillons | Statut Validation Pharma = "Validé" |
| Échantillons Livrés | Date Commande Échantillons renseignée |
| Formation IDE Théorique Planifiée | Champ "Cadre de Santé" renseigné |
| Formation IDE Théorique Réalisée | Formation IDE Théorique = "Planifiée", Champ "IDE Référente" renseigné |
| Formation IDE Pratique Réalisée | Formation IDE Théorique = "Réalisée", Formation IDE Pratique ≠ "Non planifiée" |
| Essais en Cours | Formation IDE Pratique = "Réalisée" |
| Essais Terminés | Statut Essais = "En cours" |
| Négociation Référencement | Résultat Essais = "Concluant" |

---

## Dépendances automatisations

### Champs déclencheurs

| Champ | Automatisation déclenchée |
|-------|--------------------------|
| Statut Validation Pharma = "Validé" | A10 - Commande Laphal |
| Statut Échantillons = "Livrés" | A11 - Activité formation |
| Formation IDE Pratique = "Réalisée" | A12 - Questionnaire post-formation |
| Statut Essais = "Terminés" | A13 - Questionnaires post-essais (Médecin, Pharmacien, Cadre) |
| Résultat Essais = "Non concluant - Nouvel essai" | Retour à étape "Essais en Cours" |

### Champs mis à jour automatiquement

| Automatisation | Champ mis à jour |
|----------------|------------------|
| A10 | Statut Échantillons = "Commandés", Date Commande |
| A12 | Questionnaire Satisfaction Envoyé = "Oui" |
| A13 | Questionnaire Satisfaction Envoyé = "Oui" |

---

## Bonnes pratiques

### Cohérence des données

1. **Toujours renseigner le rôle** sur les contacts avant de les lier à un deal
2. **Normaliser les noms d'établissements** pour éviter les doublons
3. **Vérifier la région** sur l'organisation pour l'assignation automatique

### Maintenance

1. **Revoir les valeurs dropdown** trimestriellement
2. **Ajouter de nouvelles spécialités** si nécessaire
3. **Mettre à jour le mapping régions** si changement de territoires

### Import de données

Lors d'un import CSV, s'assurer que :
- Les valeurs dropdown correspondent exactement aux options configurées
- Les dates sont au format attendu par Pipedrive
- Les rôles sont renseignés pour faciliter les automatisations
