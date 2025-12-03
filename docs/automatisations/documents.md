# Automatisations Documents

## A8 - Envoi automatique dossier pharmaceutique

### Objectif
Envoyer automatiquement le dossier complet au pharmacien dès qu'un deal entre en phase de validation.

### Déclencheur
- Deal Pipeline 2 passe à l'étape "Validation Pharmaceutique"

### Prérequis
- Contact avec rôle "Pharmacien" lié au deal
- Email du pharmacien renseigné
- Document PDF dossier pharma uploadé dans Pipedrive

### Actions

1. **Identifier le pharmacien**
   - Rechercher parmi les contacts liés au deal
   - Filtrer par rôle = "Pharmacien"

2. **Envoyer email avec pièce jointe**
   - Template : "Dossier Pharmaceutique"
   - PJ : Dossier_Debrichem_Pharma.pdf

3. **Créer activité de suivi**
   - Type : Tâche
   - Titre : "Relance pharma - validation dossier"
   - Échéance : J+5

4. **Ajouter note**
   - "Dossier envoyé le {date} à {email_pharmacien}"

### Template email

```
Objet : Dossier Debrichem - {organization.name} - Validation requise

Bonjour {contact.prenom},

Suite à l'intérêt manifesté par le Dr {medecin.name} du service
{deal.specialite_service}, je vous transmets le dossier complet
Debrichem pour validation par votre comité.

Vous trouverez en pièce jointe :
- Fiche technique produit
- Études cliniques
- Conditions commerciales

Je reste disponible pour tout complément d'information.

Cordialement,
{user.name}
{user.email}
{user.phone}

---
Établissement : {organization.name}
Service demandeur : {deal.specialite_service}
Médecin référent : {medecin.name}
```

### Gestion des erreurs

| Situation | Action |
|-----------|--------|
| Pas de pharmacien lié | Créer activité "Ajouter contact pharmacien au deal" |
| Email pharmacien vide | Notification "Email pharmacien manquant - compléter la fiche" |
| Envoi échoué | Notification + activité "Renvoyer dossier manuellement" |
| PJ trop volumineuse | Envoyer lien de téléchargement OneDrive |

### Configuration Pipedrive

```
Workflow :
├── Déclencheur : Deal entre dans étape "Validation Pharmaceutique"
├── Condition : Contact avec rôle "Pharmacien" existe
├── Si oui :
│   ├── Envoyer email (template + PJ)
│   ├── Créer activité (J+5)
│   └── Ajouter note
└── Si non :
    ├── Notification "Pharmacien manquant"
    └── Créer activité "Identifier pharmacien"
```

### Document à préparer

**Dossier_Debrichem_Pharma.pdf** doit contenir :
- Présentation société Debex-Médical
- Fiche technique Debrichem
- Mécanisme d'action
- Études cliniques (résumés)
- Conditions tarifaires
- Informations réglementaires (marquage CE, etc.)

Taille recommandée : < 5 Mo pour envoi par email

### Niveau de confiance
**85% - Très probable** : Fonctionne si templates bien configurés

---

## A10 - Envoi automatique commande Laphal

### Objectif
Automatiser la commande d'échantillons auprès de Laphal dès validation pharmaceutique obtenue.

### Déclencheur
- Champ "Statut Validation Pharma" passe à "Validé"

### Prérequis
- Email Laphal pour commandes confirmé
- Informations établissement complètes (adresse)
- Quantités standards définies

### Actions

1. **Générer email pré-rempli**
   - Établissement et adresse
   - Pharmacien destinataire
   - Références et quantités

2. **Envoyer à Laphal**
   - Email : [À confirmer avec Laphal]

3. **Mettre à jour le deal**
   - Statut Échantillons = "Commandés"
   - Date Commande Échantillons = aujourd'hui

4. **Créer activité de suivi**
   - Type : Tâche
   - Titre : "Suivi livraison échantillons"
   - Échéance : J+5

5. **Notifier le commercial**
   - "Commande Laphal envoyée pour {organization.name}"

### Template email Laphal

```
Objet : Commande échantillons Debrichem - {organization.name}

Bonjour,

Merci de bien vouloir procéder à l'envoi des échantillons Debrichem
pour l'établissement suivant :

═══════════════════════════════════════════════════════════════════
ÉTABLISSEMENT
═══════════════════════════════════════════════════════════════════
Nom : {organization.name}
Adresse : {organization.address}
Code postal : {organization.postal_code}
Ville : {organization.city}

═══════════════════════════════════════════════════════════════════
DESTINATAIRE
═══════════════════════════════════════════════════════════════════
Pharmacie hospitalière
À l'attention de : {pharmacien.name}

═══════════════════════════════════════════════════════════════════
COMMANDE
═══════════════════════════════════════════════════════════════════
- Debrichem [Référence 1] : X unités
- Debrichem [Référence 2] : X unités

═══════════════════════════════════════════════════════════════════
CONTEXTE
═══════════════════════════════════════════════════════════════════
Essai clinique
Service : {deal.specialite_service}
Médecin référent : Dr {medecin.name}
Commercial Debex : {user.name} ({user.email})

═══════════════════════════════════════════════════════════════════

Merci de me confirmer l'expédition par retour de mail.

Cordialement,
{user.name}
Debex-Médical
```

### Option : Validation manuelle avant envoi

Si préféré, au lieu d'envoyer directement :
1. Générer brouillon email
2. Notification "Commande Laphal à valider"
3. Commercial vérifie les données
4. Clic "Envoyer" manuel

### Données auto-remplies

| Donnée | Source Pipedrive |
|--------|------------------|
| Nom établissement | Organization > Nom |
| Adresse | Organization > Adresse |
| Pharmacien | Contact (rôle=Pharmacien) > Nom |
| Service | Deal > Spécialité Service |
| Médecin | Contact (rôle=Médecin Référent) > Nom |
| Commercial | Deal > Propriétaire |

### Quantités standards

À définir avec Debex :
- Pack essai standard : X unités
- Selon service (néphro vs réa) : ajuster ?

### Gestion des erreurs

| Situation | Action |
|-----------|--------|
| Adresse incomplète | Notification "Compléter adresse Organisation" |
| Email Laphal en erreur | Notification + activité "Commander manuellement" |

### Niveau de confiance
**80% - Très probable** : Dépend du format attendu par Laphal

---

## Documents à préparer

### Pour A8

| Document | Format | Taille max | Statut |
|----------|--------|------------|--------|
| Dossier Debrichem Pharma | PDF | 5 Mo | À préparer |
| Fiche technique | PDF | 1 Mo | À préparer |
| Études cliniques | PDF | 2 Mo | À préparer |

### Pour A10

| Information | Source | Statut |
|-------------|--------|--------|
| Email commande Laphal | À confirmer avec Laphal | À valider |
| Références produits | Catalogue Debex | À lister |
| Quantités pack essai | Commercial Debex | À définir |

---

## Alternative : Lien de téléchargement

Si le dossier pharma est trop volumineux pour un email :

1. **Stocker sur OneDrive/SharePoint** (déjà Office 365)
2. **Générer lien de partage** avec expiration
3. **Inclure le lien dans l'email** au lieu de la PJ

Template avec lien :
```
...
Vous pouvez télécharger le dossier complet via ce lien :
{lien_onedrive}

Ce lien est valable 30 jours.
...
```
