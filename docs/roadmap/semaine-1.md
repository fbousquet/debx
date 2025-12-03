# Semaine 1 - Fondations

## Objectif
Créer la structure de base dans Pipedrive : pipelines, étapes, champs personnalisés, et importer les contacts existants.

---

## Jour 1-2 : Création des Pipelines

### Pipeline 1 : Qualification Médecin

| # | Étape | Probabilité | Logique de sortie |
|---|-------|-------------|-------------------|
| 1 | Nouveau Lead | 10% | Contact identifié |
| 2 | Recherche Médecin Référent | 15% | Si entrée via pharma/cadre |
| 3 | 1er Contact Médecin | 20% | Appel/mail effectué |
| 4 | RDV Planifié | 40% | RDV Teams calé |
| 5 | Formation Théorique Réalisée | 60% | Présentation faite |
| 6 | Intéressé pour Tester | 80% | → Passe en Pipeline 2 |

**Raisons de perte Pipeline 1 :**
- Pas intéressé
- Pas le bon interlocuteur
- Réorientation autre service
- Concurrent déjà en place

### Pipeline 2 : Implémentation & Référencement

| # | Étape | Probabilité | Logique de sortie |
|---|-------|-------------|-------------------|
| 1 | Validation Pharmaceutique | 30% | Dossier envoyé, en attente |
| 2 | Commande Échantillons | 40% | Commande passée à Laphal |
| 3 | Échantillons Livrés | 50% | Produits disponibles |
| 4 | Formation IDE Planifiée | 55% | RDV cadre organisé |
| 5 | Formation IDE Réalisée | 65% | Équipes formées |
| 6 | Essais en Cours | 75% | Patients identifiés |
| 7 | Essais Terminés | 85% | Résultats collectés |
| 8 | Négociation Référencement | 90% | Discussion prix |
| 9 | Gagné - Référencé | 100% | 1ère commande |

**Raisons de perte Pipeline 2 :**
- Validation pharma refusée
- Essais non concluants
- Budget non alloué
- Changement d'interlocuteur

### Checklist création pipelines
- [ ] Pipeline 1 créé avec 6 étapes + Perdu
- [ ] Pipeline 2 créé avec 9 étapes + Perdu
- [ ] Raisons de perte configurées
- [ ] Probabilités par étape définies

---

## Jour 3 : Champs personnalisés

### Sur les Deals

| Champ | Type | Options | Obligatoire |
|-------|------|---------|-------------|
| Source Lead | Dropdown | Direct Médecin / Via Pharmacien / Via Cadre-IDE / Inbound | Oui (création) |
| Spécialité Service | Dropdown | Néphro / Réa / Autre | Oui |
| Statut Formation Théorique | Dropdown | Non planifiée / Planifiée / Réalisée | - |
| Statut Validation Pharma | Dropdown | En attente / Validé / Refusé | - |
| Date Commande Échantillons | Date | - | - |
| Statut Échantillons | Dropdown | Commandés / Expédiés / Livrés | - |
| Formation IDE | Dropdown | Non planifiée / Planifiée / Réalisée | - |
| Statut Essais | Dropdown | Non démarrés / En cours / Terminés / Concluants / Non concluants | - |
| Questionnaire Satisfaction Envoyé | Dropdown | Oui / Non | - |
| Lien RDV Directeur Médical | Texte | URL Calendly/Teams | - |

### Sur les Personnes

| Champ | Type | Options |
|-------|------|---------|
| Rôle | Dropdown | Médecin Référent / Pharmacien / Cadre de Santé / IDE Référente / IDE Backup |
| Spécialité | Texte | Libre |
| Reçoit les VM | Dropdown | Oui / Non |
| Horaires particuliers | Texte | Libre |

### Sur les Organisations

| Champ | Type | Options |
|-------|------|---------|
| Type établissement | Dropdown | CHU / CH / Clinique / EHPAD / Autre |
| Région | Dropdown | [Selon territoires commerciaux] |

### Checklist champs
- [ ] Tous les champs Deal créés
- [ ] Tous les champs Personne créés
- [ ] Tous les champs Organisation créés
- [ ] Champs obligatoires configurés

---

## Jour 4 : Configuration champs obligatoires (A17)

### Règles par étape - Pipeline 1

| Avant de passer à | Champs requis |
|-------------------|---------------|
| 1er Contact Médecin | Source Lead |
| RDV Planifié | Au moins 1 contact avec rôle "Médecin Référent" lié |
| Formation Théorique Réalisée | Lien RDV renseigné |
| Intéressé pour Tester | Statut Formation Théorique = Réalisée |

### Règles par étape - Pipeline 2

| Avant de passer à | Champs requis |
|-------------------|---------------|
| Commande Échantillons | Statut Validation Pharma = Validé |
| Échantillons Livrés | Date Commande Échantillons renseignée |
| Formation IDE Réalisée | Formation IDE = Planifiée ou Réalisée |
| Essais Terminés | Statut Essais ≠ Non démarrés |

### Checklist A17
- [ ] Règles Pipeline 1 configurées
- [ ] Règles Pipeline 2 configurées
- [ ] Tests de validation effectués

---

## Jour 5 : Import des contacts

### Préparation fichier import

Structure CSV recommandée :

```csv
Nom,Prénom,Email,Téléphone,Établissement,Service,Rôle,Spécialité,Reçoit VM
Dupont,Jean,jean.dupont@chu-xxx.fr,0600000000,CHU de XXX,Néphrologie,Médecin Référent,Néphro,Oui
Martin,Marie,marie.martin@chu-xxx.fr,0600000001,CHU de XXX,Néphrologie,IDE Référente,Néphro,Non
```

### Étapes import

1. **Export base professionnels de santé** (nephro + réa)
2. **Nettoyage données**
   - Suppression doublons évidents
   - Normalisation noms établissements
   - Vérification emails valides
3. **Import Organisations** d'abord
4. **Import Personnes** avec liaison Organisation
5. **Vérification post-import**
   - Contacts bien liés aux organisations
   - Rôles correctement assignés
   - Pas de doublons créés

### Checklist import
- [ ] Fichier CSV préparé et nettoyé
- [ ] Organisations importées
- [ ] Personnes importées
- [ ] Liaisons vérifiées
- [ ] Doublons traités

---

## Livrables fin de semaine 1

- [ ] Pipeline 1 opérationnel
- [ ] Pipeline 2 opérationnel
- [ ] Tous les champs personnalisés créés
- [ ] Champs obligatoires configurés (A17)
- [ ] Base contacts importée (nephro + réa)
- [ ] 1-2 deals tests créés pour validation

## Points d'attention

> **Important** : Ne pas déployer les automatisations tant que la structure n'est pas validée. Les automatisations dépendent des champs et pipelines.

> **Risque** : Import mal structuré = doublons + données incohérentes. Prendre le temps de bien préparer le fichier.
