# Atelier Configuration Pipedrive - 2 Janvier 2025

**Date** : Vendredi 2 janvier 2025, 14h00-16h00
**Participants** : Yann, Ben, Fabien
**Objectif** : Configurer le Module Projets Pipedrive et simplifier le CRM

---

## Ordre du jour

### 1. Découverte Module Projets (30 min)

**Objectifs** :
- Explorer l'interface du module Projets
- Comprendre les concepts : phases, tâches, modèles
- Identifier les liens avec le CRM (affaires, personnes)

**Actions** :
- [ ] Naviguer dans l'interface Projets
- [ ] Créer un projet test
- [ ] Tester l'ajout de tâches et sous-tâches
- [ ] Vérifier le lien avec les affaires existantes

**Questions à valider** :
- Peut-on assigner des tâches à des personnes externes ?
- Comment les activités CRM se synchronisent avec les projets ?
- Peut-on grouper les projets (par région, par groupe hospitalier) ?

---

### 2. Création du premier modèle de projet (45 min)

**Objectif** : Créer le modèle "Centre Hospitalier Standard"

**Étapes du projet à configurer** :

| Phase | Description | Tâches types |
|-------|-------------|--------------|
| **PROSPECT** | Contact initial | Identifier décideur, Premier contact |
| **LEAD** | Intérêt confirmé | RDV formation théorique, Définir protocole douleur |
| **ESSAIS** | Tests produit | Validation pharma, Commande échantillons, Formation IDE, Essais patients |
| **GO/NO-GO** | Décision | Évaluation résultats, Décision poursuite |
| **RÉFÉRENCEMENT** | Validation officielle | Négociation, Première commande |
| **DÉPLOIEMENT** | Extension services | Déploiement S1, S2, S3... |

**Champs personnalisés à ajouter** :
- Groupe hospitalier (dropdown)
- Région (dropdown)
- Nombre de lits (nombre)
- CA potentiel (devise)

**Actions** :
- [ ] Créer le modèle avec les 6 phases
- [ ] Ajouter les tâches types par phase
- [ ] Configurer les champs personnalisés
- [ ] Tester la création d'un projet depuis le modèle

---

### 3. Création des premiers projets réels (30 min)

**Objectif** : Créer les projets pour les établissements en cours

**Établissements prioritaires** (à confirmer avec Ben/Yann) :
1. APHP - Pitié-Salpêtrière
2. APHP - HEGP
3. APHM (Conception, Timone)
4. CHU Brest
5. Hospices Civils de Lyon
6. CHU Nîmes

**Actions par projet** :
- [ ] Créer le projet depuis le modèle
- [ ] Positionner dans la bonne phase
- [ ] Rattacher les contacts existants
- [ ] Documenter l'historique (notes)

---

### 4. Simplification du CRM (15 min)

**Objectif** : Valider la liste des champs réglementaires obligatoires

**Champs sur les Personnes** :
| Champ | Type | Obligatoire |
|-------|------|-------------|
| RPPS | Texte | Recommandé |
| Établissement | Lien Org | Oui |
| Service | Texte | Oui |
| Qualification | Dropdown | Oui |
| E32 Consentement | Dropdown | Oui |

**Types d'activités** :
- Repas
- Face-face
- Vidéo
- Téléphone
- Congrès
- Webinar

**Actions** :
- [ ] Valider la liste des champs
- [ ] Créer les champs manquants
- [ ] Configurer les types d'activités

---

## Prérequis (Fabien - avant l'atelier)

- [x] Module Projets activé
- [x] William ajouté aux utilisateurs Pipedrive
- [ ] Serveur Hostinger installé
- [ ] Vérifier que tous les participants ont accès à Pipedrive

---

## Livrables attendus

À l'issue de l'atelier :
1. **1 modèle de projet** "Centre Hospitalier" prêt à l'emploi
2. **5-6 projets créés** pour les établissements en cours
3. **Liste validée** des champs CRM réglementaires
4. **Documentation** des conventions de nommage

---

## Prochaines étapes post-atelier

### Fabien (Infrastructure)
- Finaliser installation serveur Hostinger
- Déployer N8N
- Créer interface recherche soignants
- Automatisation extraction transparence (repas)

### Yann/Ben (Opérationnel)
- Compléter les projets avec l'historique
- Former William sur le nouvel outil
- Identifier les modèles de projets supplémentaires (petit centre, EHPAD...)

---

## Notes de la réunion

*(À compléter pendant l'atelier)*

### Décisions prises

### Points en suspens

### Actions identifiées
