# Automatisations de Relance et Alertes

## A4 - Séquence relance post 1er contact

### Objectif
Éviter de perdre des prospects par manque de suivi après le premier contact.

### Déclencheur
- Deal en étape "1er Contact Médecin" (Pipeline 1)
- Aucune activité complétée depuis 3 jours

### Séquence d'actions

```
J+3 sans réponse :
├── Envoyer email "Relance 1"
├── Créer activité "Appel de suivi" (J+2)
│
J+7 sans réponse :
├── Envoyer email "Relance 2"
├── Créer activité "Appel de suivi" (J+3)
│
J+12 sans réponse :
├── Notification "Deal à qualifier ou clôturer"
└── Créer activité "Décision finale"
```

### Templates email

**Relance 1 (J+3)**
```
Objet : Suite à notre prise de contact - Debex-Médical

Bonjour {contact.prenom},

Je me permets de revenir vers vous suite à mon message précédent
concernant notre solution d'aide à la cicatrisation Debrichem.

Seriez-vous disponible pour un court échange téléphonique cette semaine ?

Je reste à votre disposition.

Cordialement,
{user.name}
{user.signature}
```

**Relance 2 (J+7)**
```
Objet : Dernière relance - Debrichem

Bonjour {contact.prenom},

N'ayant pas eu de retour de votre part, je me permets une dernière relance.

Si le sujet de la prise en charge des plaies chroniques vous intéresse,
je serais ravi de vous présenter notre approche en 15 minutes.

Dans le cas contraire, n'hésitez pas à me le signaler afin que je ne
vous importune plus.

Bien cordialement,
{user.name}
```

### Configuration Pipedrive

1. Aller dans **Automatisations** > **Créer workflow**
2. Déclencheur : "Deal entre dans une étape" + "1er Contact Médecin"
3. Condition : "Temps écoulé" = 3 jours + "Aucune activité"
4. Actions : Envoyer email + Créer activité

### Niveau de confiance
**95% - Pratiquement certain** : Best practice universelle, ROI prouvé

---

## A5 - Alerte deal stagne +7 jours

### Objectif
Alerter le commercial quand un deal n'avance plus depuis une semaine.

### Déclencheur
- Deal dans n'importe quel pipeline
- Aucun changement d'étape depuis 7 jours
- ET aucune activité complétée depuis 7 jours

### Actions
1. Notification in-app au propriétaire du deal
2. Email récapitulatif au commercial

### Message notification
```
⚠️ Deal en pause depuis 7 jours

Deal : {deal.title}
Étape : {deal.stage}
Dernière activité : {deal.last_activity_date}

Action requise : mettre à jour ou planifier une prochaine étape.
```

### Configuration
- Workflow quotidien à 9h
- Filtre : deals sans activité > 7 jours
- Action : notification + email

### Niveau de confiance
**95% - Pratiquement certain** : Hygiène pipeline de base

---

## A9 - Alerte validation pharma > 10 jours

### Objectif
Relancer quand la validation pharmaceutique traîne trop longtemps.

### Déclencheur
- Deal en étape "Validation Pharmaceutique" (Pipeline 2)
- Depuis plus de 10 jours

### Actions
1. Notification au commercial
2. Créer activité "Relancer pharmacien"
3. (Optionnel) Envoyer email de relance au pharmacien

### Template relance pharmacien (optionnel)
```
Objet : Relance - Dossier Debrichem en attente de validation

Bonjour {contact.prenom},

Je me permets de revenir vers vous concernant le dossier Debrichem
transmis le {deal.date_entree_etape} pour le service {deal.specialite_service}.

Avez-vous pu le soumettre au comité ? Y a-t-il des informations
complémentaires dont vous auriez besoin ?

Je reste disponible pour en discuter.

Cordialement,
{user.name}
```

### Configuration
- Déclencheur : temps dans étape > 10 jours
- Condition : étape = "Validation Pharmaceutique"
- Actions : notification + activité

### Niveau de confiance
**85% - Très probable** : Dépend du cycle réel de validation

---

## A14 - Alerte deal sans activité > 14 jours

### Objectif
Rapport quotidien des deals "zombies" pour maintenir un pipeline propre.

### Déclencheur
- Quotidien à 8h00

### Actions
1. Lister tous les deals sans activité > 14 jours
2. Grouper par commercial
3. Envoyer email récapitulatif

### Format rapport
```
Objet : [Quotidien] Deals sans activité - Action requise

Bonjour {user.name},

Les deals suivants n'ont eu aucune activité depuis plus de 14 jours :

PIPELINE QUALIFICATION :
• {deal.title} - Étape : {deal.stage} - Dernière activité : {deal.last_activity}
• ...

PIPELINE IMPLÉMENTATION :
• {deal.title} - Étape : {deal.stage} - Dernière activité : {deal.last_activity}
• ...

Total : {count} deals à revoir

Merci de mettre à jour ces deals ou de les clôturer si non pertinents.
```

### Configuration
- Rapport Insights ou workflow quotidien
- Filtre : last_activity > 14 jours
- Destinataires : commerciaux + manager

### Niveau de confiance
**95% - Pratiquement certain** : Standard de gestion de pipeline

---

## Paramètres recommandés

| Automatisation | Délai | Ajustable ? |
|----------------|-------|-------------|
| A4 - Relance 1 | 3 jours | Oui (2-5 jours) |
| A4 - Relance 2 | 7 jours | Oui (5-10 jours) |
| A4 - Notification finale | 12 jours | Oui (10-15 jours) |
| A5 - Alerte stagnation | 7 jours | Oui (5-10 jours) |
| A9 - Alerte pharma | 10 jours | Oui (7-14 jours selon établissement) |
| A14 - Alerte zombie | 14 jours | Oui (10-21 jours) |

---

## Bonnes pratiques

### Éviter le spam
- Maximum 2-3 relances par prospect
- Respecter les désinscriptions
- Personnaliser les messages

### Calibrer les alertes
- Si trop d'alertes → seuils trop bas → commerciaux ignorent
- Si trop peu d'alertes → deals oubliés
- Ajuster selon le cycle réel observé

### Documenter les actions
- Toujours créer une note quand on agit sur une alerte
- Permet de comprendre l'historique du deal
