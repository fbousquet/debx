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
FORMATION IDE PLANIFIÉE
    │
    ▼
FORMATION IDE RÉALISÉE
    │
    ├──► Questionnaire satisfaction (A12)
    │
    ▼
ESSAIS EN COURS
    │
    ▼
ESSAIS TERMINÉS
    │
    ├──► Questionnaires (A13)
    │
    ▼
NÉGOCIATION RÉFÉRENCEMENT
    │
    ├──► Concluant → GAGNÉ (Référencé)
    └──► Non concluant → PERDU
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

### 4. Formation IDE Planifiée

| Attribut | Valeur |
|----------|--------|
| Probabilité | 55% |
| Objectif | Organiser la formation des équipes soignantes |

**Critères d'entrée :**
- Échantillons livrés
- Cadre de santé contacté

**Actions commerciales :**
- Contacter le cadre de santé
- Définir date et lieu de formation
- Identifier IDE référente et backup
- Préparer le matériel de formation

**Interlocuteurs clés :**
- Cadre de santé
- IDE référente
- IDE backup

**Checklist planification :**
```
[ ] Date formation confirmée
[ ] Lieu identifié (salle, service)
[ ] IDE référente identifiée
[ ] IDE backup identifiée
[ ] Créneaux validés avec le service
```

**Champs à renseigner :**
- Formation IDE = "Planifiée"
- Contacts Cadre/IDE liés au deal

**Critères de sortie :**
- Formation planifiée et confirmée

---

### 5. Formation IDE Réalisée

| Attribut | Valeur |
|----------|--------|
| Probabilité | 65% |
| Objectif | Former les équipes à l'utilisation de Debrichem |

**Critères d'entrée :**
- Formation planifiée
- Date atteinte

**Contenu formation pratique :**
- Rappel des indications
- Technique d'application
- Protocole de suivi patient
- Gestion des effets
- Questions/réponses

**Actions commerciales :**
- Réaliser la formation
- Remettre les protocoles écrits
- Identifier les premiers patients pour essai
- Recueillir les premiers retours

**Automatisations actives :**
- A12 : Envoi questionnaire satisfaction formation

**Champs à renseigner :**
- Formation IDE = "Réalisée"

**Livrables formation :**
- Protocole utilisation remis
- Fiche suivi patient fournie
- Contacts support communiqués

**Critères de sortie :**
- Équipe formée et autonome
- Patients à identifier pour essais

---

### 6. Essais en Cours

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

### 7. Essais Terminés

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

**Automatisations actives :**
- A13 : Envoi questionnaires satisfaction (Médecin, Pharmacien, Cadre/IDE)

**Champs à renseigner :**
- Statut Essais = "Terminés"
- (Après analyse) = "Concluants" ou "Non concluants"

**Questions clés :**
- Efficacité observée ?
- Facilité d'utilisation ?
- Comparaison vs pratiques actuelles ?
- Intention de continuer ?

**Critères de sortie :**
- Essais concluants → Négociation
- Essais non concluants → Analyse causes, décision

---

### 8. Négociation Référencement

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

### 9. Gagné - Référencé

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
| Essais non concluants | Résultats insuffisants sur les patients |
| Budget non alloué | Établissement n'a pas les moyens |
| Changement d'interlocuteur | Le médecin référent part, successeur pas intéressé |
| Concurrent sélectionné | Un autre produit a été choisi |
| Délai trop long | Le processus a traîné, intérêt perdu |

---

## Interlocuteurs par étape

| Étape | Interlocuteur principal |
|-------|------------------------|
| Validation Pharma | Pharmacien hospitalier |
| Commande/Livraison | Pharmacien + Laphal |
| Formation IDE | Cadre de santé + IDE |
| Essais | Médecin + IDE référente |
| Négociation | Pharmacien + Direction |

---

## Durées indicatives par étape

| Étape | Durée type | Alerte si > |
|-------|------------|-------------|
| Validation Pharma | 5-10 jours | 10 jours (A9) |
| Commande → Livraison | 3-7 jours | 10 jours |
| Planification formation | 5-10 jours | 14 jours |
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

FORMATION :
[ ] Cadre contacté
[ ] Date confirmée
[ ] IDE référente identifiée
[ ] Matériel préparé

ESSAIS :
[ ] Patients identifiés
[ ] Suivi en cours
[ ] Retours collectés

RÉFÉRENCEMENT :
[ ] Proposition envoyée
[ ] Négociation en cours
[ ] Commande passée
```
