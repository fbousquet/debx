# Pipeline 2 : Implémentation & Référencement

## Objectif
Accompagner le processus depuis l'accord médecin jusqu'au référencement et première commande.

## Vue d'ensemble

```
VALIDATION PHARMACEUTIQUE
    │
    ├──► Dossier envoyé auto (A8)
    ├──► Alerte si >10j (A9)
    │
    ▼
COMMANDE ÉCHANTILLONS
    │
    ├──► Mail Laphal auto (A10)
    │
    ▼
ÉCHANTILLONS LIVRÉS
    │
    ├──► Activité formation IDE (A11)
    │
    ▼
FORMATION IDE THÉORIQUE PLANIFIÉE
    │
    ▼
FORMATION IDE THÉORIQUE RÉALISÉE
    │
    ▼
FORMATION IDE PRATIQUE RÉALISÉE
    │
    ├──► Questionnaire satisfaction (A12)
    │
    ▼
ESSAIS EN COURS
    │
    ▼
ESSAIS TERMINÉS
    │
    ├──► Questionnaires (A13) → Médecin, Pharmacien, Cadre
    │
    ├──► Concluant → NÉGOCIATION RÉFÉRENCEMENT
    │                    │
    │                    ├──► Accord → GAGNÉ (Référencé)
    │                    └──► Refus → PERDU
    │
    └──► Non concluant → Nouvel essai (retour ESSAIS EN COURS)
                    └──► Abandonné → PERDU
```

---

## Étapes détaillées

### 1. Validation Pharmaceutique

| Attribut | Valeur |
|----------|--------|
| Probabilité | 30% |
| Objectif | Obtenir la validation du pharmacien hospitalier |

**Critères d'entrée :**
- Deal converti depuis Pipeline 1
- Médecin référent a donné son accord

**Actions commerciales :**
- Identifier le pharmacien hospitalier
- Préparer le dossier pharmaceutique complet

**Automatisations actives :**
- A8 : Envoi automatique du dossier au pharmacien
- A9 : Alerte si validation > 10 jours

**Interlocuteur clé :** Pharmacien hospitalier

**Documents requis :**
- Fiche technique produit
- Études cliniques
- Conditions commerciales
- Informations réglementaires

**Champs à renseigner :**
- Contact Pharmacien lié au deal
- Statut Validation Pharma = "En attente"

**Critères de sortie :**
- Pharmacien valide → Statut = "Validé"
- Pharmacien refuse → Statut = "Refusé" (→ Perdu ou négociation)

---

### 2. Commande Échantillons

| Attribut | Valeur |
|----------|--------|
| Probabilité | 40% |
| Objectif | Passer la commande d'échantillons à Laphal |

**Critères d'entrée :**
- Statut Validation Pharma = "Validé"

**Actions commerciales :**
- Définir les quantités d'échantillons
- Confirmer l'adresse de livraison (pharmacie)

**Automatisations actives :**
- A10 : Envoi automatique email commande à Laphal

**Informations commande :**
- Établissement et adresse complète
- Nom pharmacien destinataire
- Références et quantités produits
- Médecin et service demandeur

**Champs à renseigner :**
- Statut Échantillons = "Commandés"
- Date Commande Échantillons

**Critères de sortie :**
- Confirmation expédition Laphal → passer à "Échantillons Livrés"

---

### 3. Échantillons Livrés

| Attribut | Valeur |
|----------|--------|
| Probabilité | 50% |
| Objectif | Confirmer la réception des échantillons |

**Critères d'entrée :**
- Confirmation de livraison par Laphal ou pharmacien

**Actions commerciales :**
- Confirmer réception avec le pharmacien
- Organiser le transfert vers le service
- Planifier la formation IDE

**Automatisations actives :**
- A11 : Création activité "Contacter cadre pour formation"

**Champs à renseigner :**
- Statut Échantillons = "Livrés"

**Critères de sortie :**
- Échantillons disponibles dans le service
- Formation IDE à organiser

---

### 4. Formation IDE Théorique Planifiée

| Attribut | Valeur |
|----------|--------|
| Probabilité | 55% |
| Objectif | Organiser la formation théorique des équipes soignantes |

**Critères d'entrée :**
- Échantillons livrés
- Cadre de santé contacté

**Actions commerciales :**
- Contacter le cadre de santé (RDV Cadre)
- Organiser la formation : Date, Lieu, Produit
- Identifier IDE Référente + IDE Backup DEBRICHEM
- Définir les modalités de la session de formation

**Interlocuteurs clés :**
- Cadre de santé
- IDE référente
- IDE backup

**Checklist planification :**
```
[ ] RDV Cadre effectué
[ ] Date formation confirmée
[ ] Lieu identifié (salle, service)
[ ] IDE référente identifiée
[ ] IDE backup identifiée
[ ] Modalités de session définies
```

**Champs à renseigner :**
- Formation IDE Théorique = "Planifiée"
- Date Formation IDE
- Lieu Formation IDE
- Contacts Cadre/IDE liés au deal

**Critères de sortie :**
- Formation théorique planifiée et confirmée

---

### 5. Formation IDE Théorique Réalisée

| Attribut | Valeur |
|----------|--------|
| Probabilité | 60% |
| Objectif | Former les équipes sur les aspects théoriques |

**Critères d'entrée :**
- Formation théorique planifiée
- Date atteinte

**Contenu formation théorique IDE :**
- Protocole douleur
- Protocole DEBRICHEM
- Identification des patients éligibles

**Actions commerciales :**
- Réaliser la formation théorique avec les IDE
- Valider la compréhension du protocole
- Planifier la formation pratique

**Champs à renseigner :**
- Formation IDE Théorique = "Réalisée"

**Critères de sortie :**
- IDE ont compris les protocoles
- Prêts pour la formation pratique

---

### 6. Formation IDE Pratique Réalisée

| Attribut | Valeur |
|----------|--------|
| Probabilité | 65% |
| Objectif | Former les équipes à l'utilisation pratique de Debrichem |

**Critères d'entrée :**
- Formation théorique réalisée
- Échantillons disponibles dans le service

**Contenu formation pratique :**
- Technique d'application sur cas réels
- Protocole de suivi patient
- Gestion des effets
- Questions/réponses pratiques

**Actions commerciales :**
- Réaliser la formation pratique avec le produit
- Remettre les protocoles écrits
- Identifier les premiers patients pour essai
- Recueillir les premiers retours

**Automatisations actives :**
- A12 : Envoi questionnaire satisfaction formation (aux IDE Référente + Backup)

**Champs à renseigner :**
- Formation IDE Pratique = "Réalisée"

**Livrables formation :**
- Protocole utilisation remis
- Fiche suivi patient fournie
- Contacts support communiqués

**Critères de sortie :**
- Équipe formée et autonome
- Patients à identifier pour essais

---

### 7. Essais en Cours

| Attribut | Valeur |
|----------|--------|
| Probabilité | 75% |
| Objectif | Suivre les essais sur patients |

**Critères d'entrée :**
- Formation réalisée
- Patients identifiés

**Actions commerciales :**
- Suivre le déroulement des essais
- Rester disponible pour questions
- Documenter les cas

**Champs à renseigner :**
- Statut Essais = "En cours"

**Suivi essais :**
- Nombre de patients en essai
- Durée moyenne d'essai
- Premiers retours qualitatifs

**Critères de sortie :**
- Essais terminés (suffisamment de patients traités)
- Résultats à analyser

---

### 8. Essais Terminés

| Attribut | Valeur |
|----------|--------|
| Probabilité | 85% |
| Objectif | Collecter les résultats et feedback |

**Critères d'entrée :**
- Essais réalisés sur nombre suffisant de patients

**Actions commerciales :**
- Collecter les retours médecin
- Collecter les retours équipe soignante
- Analyser les résultats
- Envoyer questionnaires satisfaction

**Automatisations actives :**
- A13 : Envoi questionnaires satisfaction à **3 destinataires** :
  - Médecin référent
  - Pharmacien
  - Cadre de santé

**Champs à renseigner :**
- Statut Essais = "Terminés"
- Résultat Essais = "Concluant" / "Non concluant - Nouvel essai" / "Non concluant - Abandonné"

**Questions clés :**
- Efficacité observée ?
- Facilité d'utilisation ?
- Comparaison vs pratiques actuelles ?
- Intention de continuer ?

**Critères de sortie :**
- Résultat = "Concluant" → Négociation Référencement
- Résultat = "Non concluant - Nouvel essai" → Retour à "Essais en Cours" (nouvelle tentative)
- Résultat = "Non concluant - Abandonné" → Perdu

> **Note** : La possibilité de refaire un essai permet de gérer les cas où les premiers essais n'ont pas été optimaux (mauvais patients, problème d'application, etc.) sans perdre tout le travail accompli.

---

### 9. Négociation Référencement

| Attribut | Valeur |
|----------|--------|
| Probabilité | 90% |
| Objectif | Finaliser les conditions de référencement |

**Critères d'entrée :**
- Essais concluants
- Volonté de l'établissement de référencer

**Actions commerciales :**
- Présenter proposition commerciale
- Négocier les conditions (prix, volumes, délais)
- Obtenir l'accord final

**Points de négociation :**
- Prix unitaire
- Volumes minimum
- Conditions de paiement
- Délais de livraison
- Formation continue

**Critères de sortie :**
- Accord obtenu → Gagné
- Négociation échouée → Perdu ou retour en arrière

---

### 10. Gagné - Référencé

| Attribut | Valeur |
|----------|--------|
| Probabilité | 100% |
| Objectif | Première commande passée |

**Critères d'entrée :**
- Accord de référencement signé
- Première commande passée

**Actions post-victoire :**
- Célébrer avec l'équipe
- Documenter le cas
- Planifier le suivi (Pipeline 3 si activé)
- Identifier opportunités extension (autres services)

---

## Raisons de perte

| Raison | Description |
|--------|-------------|
| Validation pharma refusée | Pharmacien n'approuve pas le produit |
| Essais non concluants - Abandonné | Résultats insuffisants, pas de nouvelle tentative |
| Budget non alloué | Établissement n'a pas les moyens |
| Changement d'interlocuteur | Le médecin référent part, successeur pas intéressé |
| Concurrent sélectionné | Un autre produit a été choisi |
| Délai trop long | Le processus a traîné, intérêt perdu |

> **Note** : "Essais non concluants" ne mène pas forcément à "Perdu". Si le champ "Résultat Essais" = "Non concluant - Nouvel essai", le deal peut revenir à l'étape "Essais en Cours".

---

## Interlocuteurs par étape

| Étape | Interlocuteur principal |
|-------|------------------------|
| Validation Pharma | Pharmacien hospitalier |
| Commande/Livraison | Pharmacien + Laphal |
| Formation IDE Théorique | Cadre de santé + IDE |
| Formation IDE Pratique | IDE Référente + IDE Backup |
| Essais | Médecin + IDE référente |
| Négociation | Pharmacien + Direction |

---

## Durées indicatives par étape

| Étape | Durée type | Alerte si > |
|-------|------------|-------------|
| Validation Pharma | 5-10 jours | 10 jours (A9) |
| Commande → Livraison | 3-7 jours | 10 jours |
| Formation IDE Théorique | 5-10 jours | 14 jours |
| Formation IDE Pratique | 3-7 jours | 10 jours |
| Essais | 2-4 semaines | Variable |
| Négociation | 1-2 semaines | 3 semaines |

---

## Checklist commercial par étape

```
VALIDATION PHARMA :
[ ] Pharmacien identifié et lié
[ ] Dossier envoyé (ou A8)
[ ] Relance planifiée

COMMANDE :
[ ] Validation pharma OK
[ ] Commande Laphal envoyée (ou A10)
[ ] Suivi livraison planifié

FORMATION THÉORIQUE :
[ ] RDV Cadre effectué
[ ] Date et lieu confirmés
[ ] IDE référente identifiée
[ ] IDE backup identifiée
[ ] Formation théorique réalisée

FORMATION PRATIQUE :
[ ] Échantillons dans le service
[ ] Formation pratique réalisée
[ ] Protocoles remis

ESSAIS :
[ ] Patients identifiés
[ ] Suivi en cours
[ ] Retours collectés
[ ] Résultat évalué (Concluant / Non concluant)

RÉFÉRENCEMENT :
[ ] Proposition envoyée
[ ] Négociation en cours
[ ] Commande passée
```
