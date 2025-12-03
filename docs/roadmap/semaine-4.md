# Semaine 4 - Continuité & Satisfaction

## Objectif
Mettre en place la transition automatique entre pipelines, les workflows de formation IDE, et les envois de questionnaires satisfaction via Fillout.

---

## Prérequis semaine 4

- [ ] Automatisations S3 opérationnelles
- [ ] Compte Fillout configuré
- [ ] Questionnaires satisfaction créés dans Fillout
- [ ] (Optionnel) Intégration Fillout-Pipedrive via Zapier/Make

---

## Jour 16-17 : A7 - Création automatique deal Pipeline 2

### Description
Quand un deal Pipeline 1 est marqué "Gagné" (Intéressé pour tester), créer automatiquement le deal correspondant dans Pipeline 2.

### Configuration

**Déclencheur :**
- Deal Pipeline 1 "Qualification" marqué "Gagné"
- Étape finale : "Intéressé pour Tester"

**Actions :**
1. Créer nouveau Deal dans Pipeline 2 "Implémentation"
2. Étape initiale : "Validation Pharmaceutique"
3. Copier tous les champs du deal P1 vers P2
4. Lier la même Organisation
5. Lier les mêmes contacts (Personnes)
6. Ajouter note : "Converti depuis deal #[ID P1] le [Date]"
7. Créer activité "Envoyer dossier pharma" (optionnel si A8 actif)
8. Notification au commercial "Deal converti vers Implémentation"

### Mapping des champs

| Champ P1 | Champ P2 |
|----------|----------|
| Titre | Titre (identique) |
| Organisation | Organisation |
| Contacts | Contacts |
| Source Lead | Source Lead |
| Spécialité Service | Spécialité Service |
| Statut Formation Théorique | Statut Formation Théorique |
| Valeur | Valeur |
| Notes | Notes (copiées) |

### Gestion du deal P1 après conversion

Options :
1. **Archiver** : Le deal P1 reste en "Gagné", visible dans l'historique
2. **Lier** : Ajouter champ "Deal P2 associé" avec lien vers le nouveau deal

Recommandation : Option 2 pour traçabilité complète.

### Checklist A7
- [ ] Workflow créé
- [ ] Mapping champs configuré
- [ ] Liaison Organisation/Contacts fonctionnelle
- [ ] Note de conversion ajoutée
- [ ] Test conversion complet effectué

---

## Jour 18 : A11 - Activité formation IDE

### Description
Quand les échantillons sont livrés, créer automatiquement les activités pour organiser la formation des équipes IDE.

### Configuration

**Déclencheur :**
- Champ "Statut Échantillons" passe à "Livrés"

**Actions :**
1. Créer activité "Contacter cadre pour planifier formation IDE"
   - Type : Appel
   - Échéance : J+2
   - Assigné au commercial
2. Ajouter note avec checklist formation :
```
CHECKLIST FORMATION IDE :
[ ] Date formation confirmée
[ ] Lieu identifié (service)
[ ] IDE référente identifiée
[ ] IDE backup identifiée
[ ] Matériel formation préparé
[ ] Protocole à transmettre
```
3. Notification au commercial "Échantillons livrés - organiser formation"

### Checklist A11
- [ ] Workflow créé
- [ ] Activité avec échéance créée
- [ ] Note checklist ajoutée
- [ ] Test effectué

---

## Jour 19-20 : A12 & A13 - Questionnaires Fillout

### A12 - Questionnaire post-formation IDE

**Déclencheur :**
- Champ "Formation IDE" passe à "Réalisée"

**Actions :**
1. Envoyer email avec lien questionnaire Fillout
2. Mettre à jour "Questionnaire Satisfaction Envoyé" = "Oui"
3. Créer activité "Vérifier réponses questionnaire J+7"

**Template email :**

```
Objet : Votre avis sur la formation Debrichem

Bonjour,

Suite à la formation Debrichem dispensée dans votre service,
nous souhaiterions recueillir votre retour d'expérience.

Cela ne prendra que 2 minutes :
[LIEN FILLOUT QUESTIONNAIRE FORMATION]

Votre avis nous est précieux pour améliorer nos formations.

Merci !
L'équipe Debex-Médical
```

### A13 - Questionnaire post-essais

**Déclencheur :**
- Champ "Statut Essais" passe à "Terminés"

**Actions :**
1. Identifier les 3 contacts clés (Médecin, Pharmacien, Cadre/IDE)
2. Envoyer email avec lien questionnaire à chacun
3. Créer activité "Analyser retours essais J+7"

**Template email :**

```
Objet : Retour d'expérience - Essais Debrichem

Bonjour [Prénom],

Les essais Debrichem dans votre service sont maintenant terminés.
Nous souhaiterions connaître votre évaluation.

Merci de prendre 3 minutes pour répondre à ce questionnaire :
[LIEN FILLOUT QUESTIONNAIRE ESSAIS]

Vos retours sont essentiels pour la suite du processus.

Cordialement,
L'équipe Debex-Médical
```

### Configuration Fillout

**Questionnaire Formation IDE :**
- Clarté des explications (1-5)
- Utilité du contenu (1-5)
- Qualité du formateur (1-5)
- Points à améliorer (texte libre)
- Recommanderiez-vous ? (Oui/Non)

**Questionnaire Essais :**
- Facilité d'utilisation produit (1-5)
- Efficacité observée (1-5)
- Comparaison vs pratiques actuelles (Mieux/Pareil/Moins bien)
- Intention de continuer à utiliser (Oui/Non/Peut-être)
- Commentaires (texte libre)

### Intégration Fillout → Pipedrive (Optionnel)

Via Zapier ou Make :
- Quand réponse Fillout reçue
- Mettre à jour deal avec score satisfaction
- Ajouter note avec résumé réponses

### Checklist A12 & A13
- [ ] Questionnaires créés dans Fillout
- [ ] Liens questionnaires récupérés
- [ ] Templates email configurés dans Pipedrive
- [ ] Workflows A12 et A13 créés
- [ ] (Optionnel) Intégration Zapier/Make configurée
- [ ] Tests envois effectués

---

## Jour 21 : Tests end-to-end

### Parcours complet à tester

Créer un deal test et le faire passer par TOUTES les étapes :

**Pipeline 1 :**
1. [ ] Création deal → Assignation auto (A3)
2. [ ] Pas d'activité 3j → Relance auto (A4)
3. [ ] Pas d'activité 7j → Alerte stagnation (A5)
4. [ ] Passage à "Intéressé pour Tester"
5. [ ] Marquage "Gagné" → Création deal P2 (A7)

**Pipeline 2 :**
6. [ ] Deal P2 créé avec toutes les données
7. [ ] Étape "Validation Pharma" → Email dossier (A8)
8. [ ] 10j en validation → Alerte (A9)
9. [ ] Validation OK → Commande Laphal (A10)
10. [ ] Échantillons livrés → Activité formation (A11)
11. [ ] Formation réalisée → Questionnaire (A12)
12. [ ] Essais terminés → Questionnaires (A13)

### Vérifications globales

- [ ] Aucune automation qui boucle/se déclenche en double
- [ ] Tous les emails arrivent correctement
- [ ] Toutes les notifications reçues
- [ ] Toutes les activités créées avec bonnes échéances
- [ ] Champs mis à jour automatiquement

---

## Livrables fin de semaine 4

- [ ] A7 - Conversion Pipeline 1 → 2 opérationnelle
- [ ] A11 - Workflow formation IDE opérationnel
- [ ] A12 - Questionnaire post-formation configuré
- [ ] A13 - Questionnaire post-essais configuré
- [ ] Test end-to-end validé
- [ ] Documentation utilisateur mise à jour

## Points d'attention

> **Doublons deals** : Vérifier que A7 ne crée pas de doublon si on marque "Gagné" plusieurs fois par erreur.

> **Fillout limites** : Vérifier le plan Fillout (nombre de réponses/mois).

> **Fatigue questionnaires** : Ne pas sur-solliciter les contacts. 2 questionnaires max par personne par deal.

> **Personnalisation liens** : Idéalement, inclure l'ID du deal dans le lien Fillout pour retrouver la source de la réponse.
