# Automatisations Questionnaires & Satisfaction

## A12 - Questionnaire post-formation IDE

### Objectif
Collecter les retours des équipes soignantes après chaque formation pour améliorer le processus.

### Déclencheur
- Champ "Formation IDE Pratique" passe à "Réalisée"

### Actions

1. **Envoyer email avec lien Fillout**
   - Destinataires : IDE référente + IDE backup
   - Template : "Questionnaire post-formation"

2. **Mettre à jour le deal**
   - Questionnaire Satisfaction Envoyé = "Oui"

3. **Créer activité de suivi**
   - Type : Tâche
   - Titre : "Vérifier réponses questionnaire formation"
   - Échéance : J+7

### Template email

```
Objet : Votre avis sur la formation Debrichem

Bonjour {contact.prenom},

Suite à la formation Debrichem dispensée dans votre service le {date},
nous souhaiterions recueillir votre retour d'expérience.

Cela ne vous prendra que 2 minutes :

👉 {LIEN_FILLOUT_FORMATION}

Votre avis nous est précieux pour améliorer nos formations et mieux
accompagner les équipes soignantes.

Merci pour votre participation !

Cordialement,
L'équipe Debex-Médical
```

### Questionnaire Fillout - Formation IDE

**Questions à configurer :**

| Question | Type | Options |
|----------|------|---------|
| Clarté des explications | Échelle 1-5 | 1=Pas clair ... 5=Très clair |
| Utilité du contenu | Échelle 1-5 | 1=Peu utile ... 5=Très utile |
| Qualité du formateur | Échelle 1-5 | 1=Insuffisant ... 5=Excellent |
| Durée de la formation | Choix unique | Trop courte / Adaptée / Trop longue |
| Points à améliorer | Texte libre | - |
| Ce qui vous a le plus aidé | Texte libre | - |
| Recommanderiez-vous cette formation ? | Oui/Non | - |

**Champs cachés (pour traçabilité) :**
- deal_id : {deal.id}
- organisation : {organization.name}
- commercial : {user.name}

### Niveau de confiance
**85% - Très probable** : Simple si Fillout correctement configuré

---

## A13 - Questionnaires post-essais (Multi-destinataires)

### Objectif
Collecter les retours des **3 interlocuteurs clés** après la phase d'essais pour évaluer le produit. Conformément au flowchart commercial, les questionnaires sont envoyés à 3 cibles distinctes.

### Déclencheur
- Champ "Statut Essais" passe à "Terminés"

### Destinataires (3 envois distincts)

| Destinataire | Champ Pipedrive | Perspective |
|--------------|-----------------|-------------|
| Médecin Référent | Médecin Référent (Personne) | Efficacité clinique, décision prescription |
| Pharmacien | Pharmacien Principal (Personne) | Logistique, coût, référencement |
| Cadre de Santé | Cadre de Santé (Personne) | Organisation, satisfaction équipe |

### Actions

1. **Pour CHAQUE contact clé (Médecin, Pharmacien, Cadre)** :
   - Générer lien Fillout personnalisé avec `contact_role` en paramètre
   - Envoyer email avec template adapté au rôle
   - Logger l'envoi dans les notes du deal

2. **Créer activité de suivi unique**
   - Type : Tâche
   - Titre : "Analyser retours essais (3 questionnaires)"
   - Échéance : J+7

### Template email

```
Objet : Retour d'expérience - Essais Debrichem

Bonjour {contact.prenom},

Les essais Debrichem dans le service {deal.specialite_service} de
{organization.name} sont maintenant terminés.

En tant que {contact.role}, votre évaluation est essentielle pour
nous permettre de poursuivre ensemble.

Merci de prendre 3 minutes pour répondre à ce questionnaire :

👉 {LIEN_FILLOUT_ESSAIS}

Vos retours sont précieux.

Cordialement,
{user.name}
Debex-Médical
```

### Questionnaire Fillout - Essais

**Questions à configurer :**

| Question | Type | Options |
|----------|------|---------|
| Facilité d'utilisation | Échelle 1-5 | 1=Difficile ... 5=Très facile |
| Efficacité observée | Échelle 1-5 | 1=Insuffisante ... 5=Excellente |
| Comparaison vs pratiques actuelles | Choix unique | Mieux / Équivalent / Moins bien |
| Tolérance patients | Échelle 1-5 | 1=Mauvaise ... 5=Excellente |
| Gestion de la douleur | Échelle 1-5 | 1=Inefficace ... 5=Très efficace |
| Intention de continuer à utiliser | Choix unique | Oui / Non / Peut-être |
| Recommanderiez-vous à des collègues ? | Oui/Non | - |
| Commentaires / Suggestions | Texte libre | - |

**Champs cachés :**
- deal_id : {deal.id}
- contact_role : {contact.role}
- organisation : {organization.name}

### Gestion multi-destinataires

```
POUR CHAQUE destinataire :
│
├── Médecin Référent
│   ├── Lien : fillout.com/form/{id}?deal_id={deal.id}&role=medecin
│   ├── Template : "Questionnaire Médecin"
│   └── Focus : Efficacité clinique
│
├── Pharmacien Principal
│   ├── Lien : fillout.com/form/{id}?deal_id={deal.id}&role=pharmacien
│   ├── Template : "Questionnaire Pharmacien"
│   └── Focus : Logistique, référencement
│
└── Cadre de Santé
    ├── Lien : fillout.com/form/{id}?deal_id={deal.id}&role=cadre
    ├── Template : "Questionnaire Cadre"
    └── Focus : Organisation équipe

→ 3 emails envoyés
→ 1 note récapitulative dans le deal
→ 1 activité de suivi créée
```

### Niveau de confiance
**75% - Assez probable** : Plus complexe (3 envois distincts, nécessite une boucle ou 3 actions séquentielles dans le workflow)

---

## A19 - Webhook Fillout → Pipedrive

### Objectif
Récupérer automatiquement les réponses Fillout dans Pipedrive pour centraliser les données.

### Prérequis
- Compte Zapier ou Make (Integromat)
- Accès API Pipedrive

### Architecture

```
Fillout                  Zapier/Make               Pipedrive
───────────────────────────────────────────────────────────────

[Réponse soumise] ──────► [Webhook reçu] ──────► [Mise à jour Deal]
                              │
                              ├── Parser réponses
                              ├── Identifier deal (via deal_id)
                              └── Actions :
                                  ├── Mettre à jour champs
                                  ├── Ajouter note
                                  └── Créer activité si score bas
```

### Configuration Zapier

**Trigger :** New Submission in Fillout

**Actions :**
1. **Find Deal** dans Pipedrive par ID (champ caché)
2. **Update Deal** :
   - Score satisfaction = moyenne des notes
   - Résultat essai = réponse "Intention de continuer"
3. **Create Note** avec résumé des réponses
4. **Condition** : Si score < 3 → Créer activité "Analyser feedback négatif"

### Champs à ajouter au deal

| Champ | Type | Rempli par |
|-------|------|------------|
| Score Satisfaction Formation | Nombre (1-5) | A19 (moyenne questionnaire formation) |
| Score Satisfaction Essais | Nombre (1-5) | A19 (moyenne questionnaire essais) |
| Intention Continuer | Dropdown | A19 (Oui/Non/Peut-être) |

### Format note automatique

```
═══════════════════════════════════════
RÉPONSE QUESTIONNAIRE {type}
Date : {date}
Répondant : {contact.role}
═══════════════════════════════════════

Scores :
• Facilité d'utilisation : {score}/5
• Efficacité : {score}/5
• Comparaison pratiques : {valeur}

Commentaires :
"{commentaire}"

───────────────────────────────────────
Score moyen : {moyenne}/5
```

### Niveau de confiance
**70% - Assez probable** : Dépend de la configuration Zapier/Make

---

## Configuration Fillout

### Créer les formulaires

1. **Formulaire Formation IDE**
   - URL : fillout.com/form/{id_formation}
   - Champs cachés : deal_id, organisation, commercial

2. **Formulaire Essais**
   - URL : fillout.com/form/{id_essais}
   - Champs cachés : deal_id, contact_role, organisation

### Personnalisation des liens

Pour tracer la source de chaque réponse, utiliser des paramètres URL :

```
https://fillout.com/form/{form_id}?deal_id={deal.id}&org={org.name}&role={contact.role}
```

### Notifications Fillout

Configurer Fillout pour notifier par email à chaque réponse :
- Destinataire : commercial propriétaire du deal
- Permet de réagir rapidement aux feedbacks négatifs

---

## Bonnes pratiques

### Éviter la fatigue questionnaire

- Maximum 2 questionnaires par personne par deal
- Questionnaires courts (< 3 minutes)
- Expliquer pourquoi le feedback est important

### Exploiter les réponses

- Analyser les tendances (scores moyens par établissement)
- Identifier les points d'amélioration récurrents
- Célébrer les retours positifs avec l'équipe

### Relance non-répondants

Si pas de réponse après 7 jours :
- Envoyer une relance courte
- "Votre avis compte, 2 minutes suffisent"
- Maximum 1 relance

---

## Checklist déploiement

- [ ] Formulaire Formation créé dans Fillout
- [ ] Formulaire Essais créé dans Fillout
- [ ] Champs cachés configurés
- [ ] URLs récupérées et intégrées aux templates email
- [ ] Templates email créés dans Pipedrive
- [ ] Workflows A12 et A13 configurés
- [ ] (Optionnel) Zapier/Make configuré pour A19
- [ ] Tests envoi effectués
- [ ] Réponse test reçue et vérifiée
