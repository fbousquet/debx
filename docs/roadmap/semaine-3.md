# Semaine 3 - Workflow Documents & Validation

## Objectif
Automatiser les envois de documents clés : dossier pharmaceutique, commande Laphal. Mettre en place les alertes de suivi validation.

---

## Prérequis semaine 3

- [ ] Automatisations S2 déployées et testées
- [ ] Template dossier pharmaceutique finalisé (Word/PDF)
- [ ] Email Laphal pour commandes identifié
- [ ] Template commande Laphal validé

---

## Jour 11-12 : Création des templates email

### Template "Dossier Pharmaceutique"

**À envoyer au pharmacien hospitalier pour validation.**

```
Objet : Dossier Debrichem - [Établissement] - Validation requise

Bonjour [Prénom],

Suite à l'intérêt manifesté par le Dr [Nom médecin référent] du service [Service],
je vous transmets le dossier complet Debrichem pour validation par votre comité.

Vous trouverez en pièce jointe :
- Fiche technique produit
- Études cliniques
- Conditions commerciales

Je reste disponible pour tout complément d'information.

Cordialement,
[Signature commercial]

---
Établissement : [Établissement]
Service demandeur : [Service]
Médecin référent : [Nom médecin]
```

**Pièces jointes :**
- Dossier_Debrichem_Pharma.pdf

### Template "Commande Échantillons Laphal"

**À envoyer à Laphal pour déclencher l'envoi des échantillons.**

```
Objet : Commande échantillons Debrichem - [Établissement]

Bonjour,

Merci de bien vouloir procéder à l'envoi des échantillons Debrichem pour l'établissement suivant :

ÉTABLISSEMENT :
[Nom établissement]
[Adresse complète]

DESTINATAIRE :
Pharmacie hospitalière
À l'attention de : [Nom pharmacien]

QUANTITÉS :
- Debrichem [Référence 1] : X unités
- Debrichem [Référence 2] : X unités

COMMENTAIRES :
Essai clinique service [Service] - Médecin référent : Dr [Nom]

Merci de me confirmer l'expédition.

Cordialement,
[Signature commercial]
```

### Checklist templates
- [ ] Template dossier pharma créé dans Pipedrive
- [ ] Pièce jointe dossier pharma uploadée
- [ ] Template commande Laphal créé
- [ ] Champs de fusion testés

---

## Jour 13 : A8 - Envoi automatique dossier pharmaceutique

### Description
Quand un deal entre en étape "Validation Pharmaceutique", envoyer automatiquement le dossier au pharmacien.

### Configuration

**Déclencheur :**
- Deal Pipeline 2 passe à l'étape "Validation Pharmaceutique"

**Conditions préalables :**
- Contact avec rôle "Pharmacien" lié au deal
- Email du pharmacien renseigné

**Actions :**
1. Identifier le contact "Pharmacien" lié
2. Envoyer email template "Dossier Pharmaceutique"
3. Joindre le PDF dossier
4. Créer activité "Relance pharma J+5"
5. Mettre à jour note : "Dossier envoyé le [Date] à [Email]"

### Gestion erreurs

| Situation | Action |
|-----------|--------|
| Pas de pharmacien lié | Créer activité "Ajouter contact pharmacien" |
| Email pharmacien vide | Notification "Email pharmacien manquant" |
| Envoi échoué | Notification + activité "Renvoyer dossier" |

### Checklist A8
- [ ] Workflow créé
- [ ] Condition "rôle pharmacien" configurée
- [ ] Email avec PJ testé
- [ ] Activité relance créée automatiquement
- [ ] Gestion erreurs configurée

---

## Jour 14 : A10 - Envoi automatique commande Laphal

### Description
Quand la validation pharma est confirmée, envoyer automatiquement la commande échantillons à Laphal.

### Configuration

**Déclencheur :**
- Champ "Statut Validation Pharma" passe à "Validé"

**Actions :**
1. Générer email pré-rempli avec infos du deal
2. Envoyer à [email-commande@laphal.com] (à confirmer)
3. Mettre à jour "Statut Échantillons" = "Commandés"
4. Mettre à jour "Date Commande Échantillons" = aujourd'hui
5. Créer activité "Suivi livraison J+5"
6. Notification au commercial "Commande Laphal envoyée"

### Données à inclure automatiquement

| Donnée | Source Pipedrive |
|--------|------------------|
| Établissement | Organisation.Nom |
| Adresse | Organisation.Adresse |
| Pharmacien | Contact (rôle=Pharmacien).Nom |
| Service | Deal.Spécialité Service |
| Médecin | Contact (rôle=Médecin Référent).Nom |

### Option : Validation manuelle
Si préféré, au lieu d'envoyer directement :
1. Générer brouillon email
2. Notification "Commande Laphal à valider"
3. Commercial vérifie et clique "Envoyer"

### Checklist A10
- [ ] Email Laphal confirmé
- [ ] Workflow créé
- [ ] Champs auto-remplis testés
- [ ] Mise à jour statuts automatique
- [ ] Activité suivi créée

---

## Jour 15 : A9 - Alerte validation pharma > 10 jours

### Description
Si un deal reste en "Validation Pharmaceutique" plus de 10 jours, alerter pour relancer.

### Configuration

**Déclencheur :**
- Deal en étape "Validation Pharmaceutique"
- Depuis plus de 10 jours

**Actions :**
1. Notification au commercial
2. Créer activité "Relancer pharmacien - validation en attente"
3. (Optionnel) Envoyer email de relance au pharmacien

### Template relance pharmacien (optionnel)

```
Objet : Relance - Dossier Debrichem en attente de validation

Bonjour [Prénom],

Je me permets de revenir vers vous concernant le dossier Debrichem transmis
le [Date envoi] pour le service [Service].

Avez-vous pu le soumettre au comité ? Y a-t-il des informations complémentaires
dont vous auriez besoin ?

Je reste disponible pour en discuter.

Cordialement,
[Signature]
```

### Checklist A9
- [ ] Workflow créé
- [ ] Délai 10 jours configuré
- [ ] Notification testée
- [ ] Activité créée automatiquement
- [ ] (Optionnel) Email relance configuré

---

## Tests de validation fin S3

### Scénarios à tester

1. **Déplacer deal vers "Validation Pharmaceutique"**
   - [ ] Email dossier envoyé au pharmacien ?
   - [ ] PJ attachée ?
   - [ ] Activité relance J+5 créée ?

2. **Passer "Statut Validation Pharma" à "Validé"**
   - [ ] Email commande Laphal envoyé ?
   - [ ] Statut Échantillons = "Commandés" ?
   - [ ] Date commande renseignée ?
   - [ ] Activité suivi J+5 créée ?

3. **Laisser deal 10+ jours en validation pharma**
   - [ ] Alerte reçue ?
   - [ ] Activité relance créée ?

4. **Test erreur : deal sans pharmacien lié**
   - [ ] Notification d'erreur reçue ?
   - [ ] Activité "Ajouter pharmacien" créée ?

---

## Livrables fin de semaine 3

- [ ] Templates email documents créés
- [ ] A8 - Envoi dossier pharma opérationnel
- [ ] A10 - Commande Laphal opérationnelle
- [ ] A9 - Alerte 10 jours opérationnelle
- [ ] Tous les workflows testés
- [ ] Gestion des erreurs fonctionnelle

## Points d'attention

> **Pièces jointes** : Vérifier que le PDF dossier pharma ne dépasse pas les limites email Pipedrive. Si trop lourd, utiliser un lien de téléchargement (OneDrive/SharePoint).

> **Email Laphal** : Valider le format exact attendu par Laphal pour les commandes. Peut nécessiter ajustements.

> **RGPD** : Les emails automatiques doivent inclure les mentions légales si requis.
