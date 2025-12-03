# Automatisations Data Quality

## A1 - Création automatique Organisation depuis Personne

### Objectif
Quand un contact est créé avec un nom d'établissement, créer automatiquement l'Organisation correspondante.

### Déclencheur
- Création d'une Personne avec champ "Établissement" renseigné

### Actions
1. Vérifier si Organisation avec ce nom existe déjà
2. Si non → Créer l'Organisation
3. Lier la Personne à l'Organisation

### Logique

```
Création Personne
    │
    ├── Champ "Établissement" vide ?
    │   └── Oui → Fin (pas d'action)
    │
    └── Non → Rechercher Organisation par nom
            │
            ├── Existe → Lier Personne à Organisation existante
            │
            └── N'existe pas → Créer Organisation
                              └── Lier Personne
```

### Risques
- Doublons si orthographe différente (ex: "CHU Lyon" vs "CHU de Lyon")
- Mitigation : normaliser les noms ou utiliser A16

### Niveau de confiance
**80% - Très probable** : Utile si import massif de contacts

---

## A2 - Liaison automatique contacts établissement

### Objectif
Quand un deal est créé, proposer/lier automatiquement tous les contacts de la même Organisation.

### Déclencheur
- Création d'un Deal avec Organisation liée

### Actions
1. Récupérer tous les contacts de l'Organisation
2. Filtrer par rôle pertinent (Médecin, Pharmacien, Cadre, IDE)
3. Les ajouter comme participants au deal

### Configuration

```
Création Deal avec Organisation
    │
    └── Rechercher Personnes liées à Organisation
        │
        └── Pour chaque Personne avec rôle pertinent :
            └── Ajouter comme participant au Deal
```

### Filtrage par rôle
Ne pas lier automatiquement :
- Contacts sans rôle défini
- Contacts marqués "Inactif"

### Niveau de confiance
**75% - Assez probable** : Peut créer trop de liaisons si grosse structure

---

## A3 - Assignation automatique par territoire

### Objectif
Assigner automatiquement les nouveaux leads au commercial responsable du territoire.

### Déclencheur
- Création d'un Deal dans Pipeline 1 "Qualification"

### Configuration

**Mapping territoires → Commerciaux**

| Région | Commercial | Email |
|--------|------------|-------|
| Île-de-France | [Nom 1] | commercial1@debex.fr |
| Grand-Est | [Nom 2] | commercial2@debex.fr |
| Sud | [Nom 3] | commercial3@debex.fr |
| Autres | [Par défaut] | default@debex.fr |

> **À compléter** avec les vrais territoires et commerciaux Debex

### Actions
1. Lire le champ "Région" sur l'Organisation liée
2. Appliquer le mapping Région → Commercial
3. Assigner le deal au commercial
4. Notification "Nouveau lead assigné"

### Règle fallback

Si région non renseignée ou inconnue :
- Assigner au commercial par défaut
- Créer activité "Compléter région établissement"
- Notification "Lead assigné par défaut - région manquante"

### Configuration Pipedrive

```
Workflow :
├── Déclencheur : Deal créé dans Pipeline Qualification
├── Lire : Organization > Région
├── Condition :
│   ├── Si Région = Île-de-France → Owner = Commercial 1
│   ├── Si Région = Grand-Est → Owner = Commercial 2
│   ├── Si Région = Sud → Owner = Commercial 3
│   └── Sinon → Owner = Commercial par défaut + Activité
└── Notification au nouveau propriétaire
```

### Niveau de confiance
**90% - Très probable** : Standard, fonctionne bien si régions renseignées

---

## A16 - Détection et alerte doublons

### Objectif
Éviter les doublons de contacts et alerter pour fusion si nécessaire.

### Déclencheur
- Création d'une Personne

### Actions
1. Rechercher contacts existants avec même email OU même téléphone
2. Si match trouvé → Alerter pour vérification/fusion

### Logique

```
Création Personne (email, téléphone)
    │
    ├── Recherche par email (si renseigné)
    │   └── Match ? → Alerte doublon
    │
    └── Recherche par téléphone (si renseigné)
        └── Match ? → Alerte doublon
```

### Format alerte

```
⚠️ Doublon potentiel détecté

Nouveau contact : {nouvelle_personne.nom}
Email : {email}
Téléphone : {tel}

Contact existant similaire :
• {personne_existante.nom} (ID: {id})
• Email : {email_existant}
• Organisation : {org_existante}

Action requise : Vérifier et fusionner si nécessaire
```

### Règles de matching

| Critère | Seuil |
|---------|-------|
| Email identique | Match certain |
| Téléphone identique | Match probable |
| Nom + Organisation | Match à vérifier |

### Niveau de confiance
**85% - Très probable** : Pipedrive a une détection native, peut être renforcée

---

## A17 - Champs obligatoires avant progression

### Objectif
Garantir la qualité des données en empêchant de progresser sans les informations clés.

### Configuration native Pipedrive
Pipedrive permet de définir des champs obligatoires par étape.

### Règles Pipeline 1 - Qualification

| Avant de passer à | Champs obligatoires |
|-------------------|---------------------|
| 1er Contact Médecin | Source Lead |
| RDV Planifié | Contact "Médecin Référent" lié |
| Formation Théorique Réalisée | Lien RDV Directeur Médical |
| Intéressé pour Tester | Statut Formation Théorique = "Réalisée" |

### Règles Pipeline 2 - Implémentation

| Avant de passer à | Champs obligatoires |
|-------------------|---------------------|
| Validation Pharmaceutique | Contact "Pharmacien" lié |
| Commande Échantillons | Statut Validation Pharma = "Validé" |
| Échantillons Livrés | Date Commande Échantillons |
| Formation IDE Réalisée | Formation IDE ≠ "Non planifiée" |
| Essais Terminés | Statut Essais ≠ "Non démarrés" |

### Configuration dans Pipedrive

1. Aller dans **Paramètres** > **Pipelines**
2. Sélectionner le pipeline
3. Pour chaque étape, définir les "Champs requis"
4. Activer "Empêcher le passage si champs manquants"

### Message d'erreur type

```
Impossible de déplacer le deal vers "{étape_cible}"

Champs manquants :
• {champ_1}
• {champ_2}

Veuillez compléter ces informations avant de continuer.
```

### Niveau de confiance
**95% - Pratiquement certain** : Fonctionnalité native Pipedrive

---

## A18 - Notification échec automation (Monitoring)

### Objectif
Être alerté quand une automatisation échoue pour corriger rapidement.

### Déclencheur
- Erreur dans un workflow Pipedrive
- Échec d'envoi email
- Erreur Zapier/Make

### Configuration

**Pour Pipedrive natif :**
- Activer les notifications d'erreur workflow
- Destinataire : administrateur CRM

**Pour Zapier/Make :**
- Configurer les alertes d'échec de Zap/Scenario
- Email + notification Slack si disponible

### Format alerte

```
🚨 Échec automatisation

Workflow : {nom_workflow}
Deal concerné : {deal.title} (ID: {deal.id})
Date/Heure : {timestamp}
Erreur : {message_erreur}

Action requise : Vérifier et relancer manuellement si nécessaire
```

### Niveau de confiance
**75% - Assez probable** : Dépend des outils utilisés

---

## Synthèse priorités Data Quality

| Priorité | ID | Nom | Impact |
|----------|----|----|--------|
| 1 | A17 | Champs obligatoires | Fondation pour tout le reste |
| 2 | A3 | Assignation territoire | Zéro lead orphelin |
| 3 | A16 | Détection doublons | Base propre |
| 4 | A1 | Création auto Organisation | Gain de temps |
| 5 | A2 | Liaison contacts | Vision 360° |

---

## Maintenance Data Quality

### Hebdomadaire
- Vérifier les contacts sans Organisation
- Vérifier les deals sans propriétaire
- Traiter les alertes doublons en attente

### Mensuelle
- Audit des champs vides critiques
- Nettoyage des contacts obsolètes
- Revue des raisons de perte

### Trimestrielle
- Revue des valeurs dropdown (ajouts nécessaires ?)
- Mise à jour mapping territoires si changements
- Formation équipe sur bonnes pratiques saisie
