# Semaine 2 - Quick Wins Automatisations

## Objectif
Déployer les automatisations à fort impact et faible complexité : relances, alertes de stagnation, assignation automatique.

---

## Prérequis semaine 2

- [ ] Pipeline 1 et 2 créés (S1)
- [ ] Champs personnalisés configurés (S1)
- [ ] Contacts importés avec rôles (S1)
- [ ] Règles territoires commerciaux définies

---

## Jour 6-7 : A4 - Séquence relance post 1er contact

### Description
Si un deal reste en "1er Contact Médecin" sans réponse pendant 3 jours, déclencher une séquence de relances automatiques.

### Configuration Workflow Pipedrive

**Déclencheur :**
- Deal en étape "1er Contact Médecin"
- ET aucune activité complétée depuis 3 jours

**Actions :**

```
J+3 sans réponse :
├── Envoyer email template "Relance 1"
└── Créer activité "Appel de suivi" pour J+2

J+7 sans réponse :
├── Envoyer email template "Relance 2"
└── Créer activité "Appel de suivi" pour J+3

J+12 sans réponse :
├── Notification au commercial "Deal à qualifier ou clôturer"
└── Créer activité "Décision : poursuivre ou archiver"
```

### Template Email "Relance 1"

```
Objet : Suite à notre prise de contact - Debex-Médical

Bonjour [Prénom],

Je me permets de revenir vers vous suite à mon message précédent concernant notre solution d'aide à la cicatrisation Debrichem.

Seriez-vous disponible pour un court échange téléphonique cette semaine ?

Je reste à votre disposition.

Cordialement,
[Signature commercial]
```

### Template Email "Relance 2"

```
Objet : Dernière relance - Debrichem

Bonjour [Prénom],

N'ayant pas eu de retour de votre part, je me permets une dernière relance.

Si le sujet de la prise en charge des plaies chroniques vous intéresse, je serais ravi de vous présenter notre approche en 15 minutes.

Dans le cas contraire, n'hésitez pas à me le signaler afin que je ne vous importune plus.

Bien cordialement,
[Signature commercial]
```

### Checklist A4
- [ ] Workflow créé dans Pipedrive
- [ ] Template "Relance 1" configuré
- [ ] Template "Relance 2" configuré
- [ ] Délais validés (3j, 7j, 12j)
- [ ] Test sur deal fictif

---

## Jour 8 : A5 - Alerte deal stagne +7 jours

### Description
Alerter le commercial si un deal n'a pas changé d'étape ET n'a aucune activité depuis 7 jours.

### Configuration

**Déclencheur :**
- Deal dans n'importe quel pipeline
- Aucun changement d'étape depuis 7 jours
- Aucune activité complétée depuis 7 jours

**Actions :**
1. Notification in-app au propriétaire du deal
2. Email au commercial avec liste des deals concernés

### Message notification

```
Deal en pause depuis 7 jours : [Nom du deal]
Étape actuelle : [Étape]
Dernière activité : [Date]

Action requise : mettre à jour le deal ou planifier une prochaine étape.
```

### Checklist A5
- [ ] Workflow créé
- [ ] Message notification rédigé
- [ ] Test effectué

---

## Jour 9 : A14 - Alerte deal sans activité > 14 jours

### Description
Rapport quotidien des deals "zombies" sans aucune activité depuis 14 jours.

### Configuration

**Déclencheur :**
- Quotidien à 8h00

**Actions :**
1. Lister tous les deals sans activité > 14 jours
2. Grouper par commercial
3. Envoyer email récapitulatif

### Format rapport email

```
Objet : [Quotidien] Deals sans activité - Action requise

Bonjour,

Les deals suivants n'ont eu aucune activité depuis plus de 14 jours :

PIPELINE QUALIFICATION :
• [Deal 1] - Étape : [X] - Dernière activité : [Date]
• [Deal 2] - Étape : [X] - Dernière activité : [Date]

PIPELINE IMPLÉMENTATION :
• [Deal 3] - Étape : [X] - Dernière activité : [Date]

Merci de mettre à jour ces deals ou de les clôturer si non pertinents.
```

### Checklist A14
- [ ] Workflow ou rapport Insights créé
- [ ] Email quotidien configuré
- [ ] Destinataires définis (commerciaux + manager)
- [ ] Test effectué

---

## Jour 10 : A3 - Assignation automatique par territoire

### Description
Quand un lead arrive, l'assigner automatiquement au commercial responsable selon la région.

### Mapping territoires

| Région | Commercial assigné |
|--------|-------------------|
| Île-de-France | [Commercial 1] |
| Nord-Est | [Commercial 2] |
| Sud-Ouest | [Commercial 3] |
| ... | ... |

> **Note** : Compléter ce mapping avec les règles réelles définies par Debex.

### Configuration

**Déclencheur :**
- Création d'un Deal dans Pipeline 1 "Qualification"

**Actions :**
1. Lire le champ "Région" sur l'Organisation liée
2. Appliquer le mapping territoire → Commercial
3. Assigner le deal au commercial correspondant
4. Notification au commercial "Nouveau lead assigné"

### Règle fallback
Si région non renseignée ou inconnue → Assigner au commercial par défaut (à définir) + créer activité "Compléter région".

### Checklist A3
- [ ] Mapping territoires validé avec Debex
- [ ] Workflow créé
- [ ] Règle fallback configurée
- [ ] Test avec différentes régions

---

## Tests de validation fin S2

### Scénarios à tester

1. **Créer un deal sans activité pendant 3 jours**
   - [ ] Relance 1 envoyée ?
   - [ ] Activité créée ?

2. **Créer un deal sans activité pendant 7 jours**
   - [ ] Alerte stagnation reçue ?

3. **Créer un deal dans nouvelle région**
   - [ ] Commercial assigné correctement ?

4. **Vérifier rapport quotidien 14j**
   - [ ] Email reçu ?
   - [ ] Deals listés correctement ?

---

## Livrables fin de semaine 2

- [ ] A4 - Séquence relance opérationnelle
- [ ] A5 - Alerte 7 jours opérationnelle
- [ ] A14 - Rapport 14 jours opérationnel
- [ ] A3 - Assignation territoire opérationnelle
- [ ] Templates email créés et testés
- [ ] Tous les workflows testés sur deals fictifs

## Points d'attention

> **Spam** : Limiter les relances à 2-3 max pour éviter d'irriter les prospects.

> **Surcharge notifications** : Si trop d'alertes, les commerciaux vont les ignorer. Calibrer les seuils (7j, 14j) selon le cycle réel.

> **Mapping territoires** : S'assurer que chaque région possible est couverte, sinon prévoir un fallback.
