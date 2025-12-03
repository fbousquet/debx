# Pipeline 1 : Qualification Médecin

## Objectif
Amener le médecin référent à accepter de tester le produit Debrichem.

## Vue d'ensemble

```
NOUVEAU LEAD
    │
    ▼
RECHERCHE MÉDECIN RÉFÉRENT (si entrée via pharma/cadre)
    │
    ▼
1ER CONTACT MÉDECIN
    │
    ├──► Pas de réponse → Relances auto (A4)
    │
    ▼
RDV PLANIFIÉ
    │
    ▼
FORMATION THÉORIQUE RÉALISÉE
    │
    ├──► Pas intéressé → PERDU
    │
    ▼
INTÉRESSÉ POUR TESTER → GAGNÉ → Pipeline 2
```

---

## Étapes détaillées

### 1. Nouveau Lead

| Attribut | Valeur |
|----------|--------|
| Probabilité | 10% |
| Objectif | Contact identifié, deal créé |

**Critères d'entrée :**
- Nouveau contact identifié (médecin, pharmacien ou cadre)
- Organisation renseignée
- Source du lead identifiée

**Actions commerciales :**
- Qualifier le contact
- Si source = Pharmacien ou Cadre-IDE → identifier le médecin référent
- Si contact intermédiaire → renseigner le champ "Contact Intermédiaire"
- Rechercher informations établissement

**Champs obligatoires :**
- Source Lead (avec distinction Direct/Inbound par type de contact)

**Champs recommandés :**
- Contact Intermédiaire (si entrée via pharmacien/cadre/IDE)

---

### 2. Recherche Médecin Référent

| Attribut | Valeur |
|----------|--------|
| Probabilité | 15% |
| Objectif | Identifier le bon interlocuteur médecin |

**Critères d'entrée :**
- Source Lead = "Via Pharmacien" ou "Via Cadre-IDE"
- Médecin référent non encore identifié

**Actions commerciales :**
- Demander au contact initial le nom du médecin référent
- Rechercher dans la base
- Créer le contact médecin si inexistant

**Critères de sortie :**
- Contact "Médecin Référent" lié au deal

> **Note** : Si source = "Direct Médecin", cette étape est sautée.

---

### 3. 1er Contact Médecin

| Attribut | Valeur |
|----------|--------|
| Probabilité | 20% |
| Objectif | Établir le premier contact avec le médecin |

**Critères d'entrée :**
- Médecin référent identifié et lié au deal

**Actions commerciales :**
- Appel ou email d'introduction
- Présentation rapide Debex/Debrichem
- Proposition de RDV pour formation

**Automatisations actives :**
- A4 : Si pas de réponse J+3 → Relance auto
- A4 : Si pas de réponse J+7 → 2ème relance
- A5 : Alerte si stagnation > 7 jours

**Critères de sortie :**
- Médecin accepte un RDV
- OU : Médecin refuse (→ Perdu)

---

### 4. RDV Planifié

| Attribut | Valeur |
|----------|--------|
| Probabilité | 40% |
| Objectif | RDV confirmé avec le médecin |

**Critères d'entrée :**
- Médecin a accepté le principe d'un RDV
- Contact "Médecin Référent" lié

**Actions commerciales :**
- Envoyer lien Teams/Calendly
- Confirmer date et heure
- Préparer la présentation formation théorique

**Champs à renseigner :**
- Lien RDV Directeur Médical

**Critères de sortie :**
- RDV effectué (→ Formation Théorique)
- OU : RDV annulé/reporté (reste dans cette étape)
- OU : Médecin se désiste (→ Perdu)

---

### 5. Formation Théorique Réalisée

| Attribut | Valeur |
|----------|--------|
| Probabilité | 60% |
| Objectif | Médecin a reçu la présentation complète |

**Critères d'entrée :**
- RDV effectué
- Lien RDV renseigné

**Contenu formation théorique :**
- Problématique plaies/cicatrisation
- Biofilm et ses impacts
- Solution Debrichem
- Gestion de la douleur
- Études cliniques
- Indication de patients pour essai

**Actions commerciales :**
- Réaliser la formation (Directeur Médical via Teams)
- Répondre aux questions
- Identifier des patients potentiels pour essai
- **Définir le protocole douleur** avec le médecin
- **Demander l'autorisation d'utiliser le protocole douleur dans d'autres centres**
- Obtenir la décision du médecin

**Champs à mettre à jour :**
- Statut Formation Théorique = "Réalisée"
- Protocole Douleur Défini = "Oui" (si défini)
- Autorisation Multi-Centres = "Accordée" / "Refusée" / "En attente"

> **Point stratégique** : L'autorisation multi-centres pour le protocole douleur est un actif clé permettant de répliquer le protocole dans d'autres établissements sans refaire tout le travail de définition.

**Critères de sortie :**
- Médecin intéressé pour tester (→ Intéressé)
- Médecin pas convaincu (→ Perdu)
- Médecin réfléchit (relance à planifier)

---

### 6. Intéressé pour Tester

| Attribut | Valeur |
|----------|--------|
| Probabilité | 80% |
| Objectif | Médecin a donné son accord pour des essais |

**Critères d'entrée :**
- Formation théorique réalisée
- Médecin a exprimé son intérêt

**Actions commerciales :**
- Confirmer l'engagement du médecin
- Identifier le pharmacien hospitalier
- Préparer la transition vers Pipeline 2

**Automatisations actives :**
- A6 : Notification équipe "Médecin OK pour tester"

**Critères de sortie :**
- Marquer le deal "Gagné"
- → Déclenche A7 : Création auto du deal Pipeline 2

---

## Raisons de perte

| Raison | Description |
|--------|-------------|
| Pas intéressé | Le médecin n'est pas convaincu par le produit |
| Pas le bon interlocuteur | Le contact initial ne peut pas décider |
| Réorientation autre service | Produit plus adapté à un autre service |
| Concurrent déjà en place | Utilise déjà une solution concurrente |
| Budget/priorités | L'établissement a d'autres priorités |
| Injoignable | Impossible d'établir le contact après plusieurs tentatives |

---

## Métriques de suivi

| Métrique | Description |
|----------|-------------|
| Taux de conversion Lead → RDV | % de leads qui obtiennent un RDV |
| Taux de conversion RDV → Intéressé | % de RDV qui aboutissent à un intérêt |
| Durée moyenne par étape | Temps passé dans chaque étape |
| Raisons de perte | Distribution des causes d'échec |

---

## Checklist commercial par deal

```
NOUVEAU LEAD :
[ ] Source lead renseignée (avec distinction Direct/Inbound)
[ ] Organisation liée
[ ] Service/spécialité identifié
[ ] Contact intermédiaire renseigné (si applicable)

QUALIFICATION :
[ ] Médecin référent identifié
[ ] Contact médecin créé et lié
[ ] Premier contact effectué

RDV :
[ ] Lien Teams/Calendly envoyé
[ ] Date confirmée
[ ] Présentation préparée

POST-FORMATION :
[ ] Formation théorique complétée
[ ] Protocole douleur défini
[ ] Autorisation multi-centres demandée
[ ] Intérêt médecin confirmé
[ ] Pharmacien identifié pour suite
```
