# Analyse de Faisabilité - Automatisations Pipedrive

> Recherche effectuée le 23/12/2024 sur les capacités natives Pipedrive + intégrations Zapier/Make

---

## Résumé Exécutif

| Catégorie | Faisable Natif | Via Zapier/Make | Limitation majeure |
|-----------|----------------|-----------------|-------------------|
| Data Quality (A1-A3, A16-A17) | Partiel | Oui | A17 contournable via API |
| Relances (A4-A5, A9, A14) | Partiel | Oui | Rotting ≠ trigger natif |
| Workflows (A6-A7, A11, A18) | Oui | - | - |
| Documents (A8, A10) | Oui | - | Email = compte sync |
| Satisfaction (A12-A13, A19) | Oui | Oui | Fillout natif ou Zapier |

**Verdict global : 17/19 automatisations faisables nativement, 2 nécessitent workaround**

---

## Plan Pipedrive Requis

Basé sur les fonctionnalités requises :

| Fonctionnalité | Plan minimum |
|----------------|--------------|
| Workflow Automation (base) | Advanced |
| Required Fields (A17) | **Professional** |
| Automations avancées | Professional |
| Nombre d'automatisations | Advanced = 30, Pro = 60 |

**Recommandation : Plan Professional** pour avoir les Required Fields + marge sur le nombre d'automatisations.

---

## Analyse Détaillée par Automatisation

### A1 - Distribution Leads
| Aspect | Évaluation |
|--------|------------|
| **Faisabilité** | ✅ 95% - Natif Pipedrive |
| **Trigger** | "Deal created" |
| **Action** | "Update deal" → Champ Owner basé sur règle |
| **Limitation** | Règles de distribution par région = via conditions dans l'automation |
| **Alternative** | Zapier multi-step pour logique complexe |

**Source** : [Pipedrive Workflow Automation](https://support.pipedrive.com/en/article/workflow-automation)

---

### A2 - Enrichissement Contact
| Aspect | Évaluation |
|--------|------------|
| **Faisabilité** | ✅ 90% - Natif Pipedrive |
| **Trigger** | "Person created" |
| **Action** | "Update person" → Auto-compléter champs |
| **Limitation** | Enrichissement externe (Clearbit, etc.) = via Zapier |
| **Note** | Fonctionne pour mapper spécialité basée sur organisation |

---

### A3 - Normalisation Organisation
| Aspect | Évaluation |
|--------|------------|
| **Faisabilité** | ⚠️ 70% - Limité nativement |
| **Problème** | Pipedrive ne peut pas transformer du texte (majuscules, formatage) |
| **Solution** | Zapier avec Formatter by Zapier pour normaliser |
| **Alternative** | Make avec module Text parser |

---

### A4 - Séquence Relance Post 1er Contact ⭐⭐
| Aspect | Évaluation |
|--------|------------|
| **Faisabilité** | ✅ 95% - Natif Pipedrive |
| **Trigger** | "Deal stage changed to 1er Contact" |
| **Actions séquentielles** | Delay 3 jours → Email 1 → Delay 4 jours → Email 2 |
| **Condition** | "Check if email was replied to" avant 2ème envoi |
| **Limitation** | Délai max = 12 semaines (84 jours) - OK pour ce cas |

**Source** : [Pipedrive Automations Emails](https://support.pipedrive.com/en/article/automations-emails)

---

### A5 - Alerte Deal Stagne +7j ⭐⭐
| Aspect | Évaluation |
|--------|------------|
| **Faisabilité** | ⚠️ 75% - Workaround requis |
| **Problème** | Le "Rotting" Pipedrive **ne déclenche PAS d'automation** |
| **Workaround 1** | Créer activité de suivi J+7 quand deal entre dans l'étape, puis automation sur "Activity due" |
| **Workaround 2** | Utiliser app tierce [DealFlow](https://www.pipedrive.com/en/marketplace/app/deal-flow-notification-system/96a1b0d3be182e08) |
| **Workaround 3** | Zapier avec trigger "Updated Deal" + filtre sur date |

**Source** : [Pipedrive Rotting Feature](https://support.pipedrive.com/en/article/the-rotting-feature)

> ⚠️ **Point d'attention** : Cette limitation est importante car les alertes stagnation sont au coeur du processus commercial.

---

### A6 - Notification "Médecin OK pour Tester"
| Aspect | Évaluation |
|--------|------------|
| **Faisabilité** | ✅ 100% - Natif Pipedrive |
| **Trigger** | "Deal stage changed to Intéressé pour Tester" |
| **Action** | "Send email" à l'équipe (template partagé) |
| **Bonus** | Peut aussi créer une note de célébration |

---

### A7 - Création Deal Pipeline 2 ⭐
| Aspect | Évaluation |
|--------|------------|
| **Faisabilité** | ✅ 95% - Natif Pipedrive |
| **Trigger** | "Deal status changed to Won" |
| **Action** | "Create deal" dans Pipeline 2 |
| **Données copiées** | Organisation, Contact, Valeur, Champs custom (via variables) |
| **Limitation** | Échoue si limite de deals atteinte |

**Source** : [Pipedrive Community - Create deal on Won](https://community.pipedrive.com/discussion/4707/create-deal-in-another-pipeline-automatically-when-marked-as-won)

---

### A8 - Envoi Auto Dossier Pharma ⭐
| Aspect | Évaluation |
|--------|------------|
| **Faisabilité** | ✅ 90% - Natif Pipedrive |
| **Trigger** | "Deal stage changed to Validation Pharmaceutique" |
| **Action** | "Send email using template" au pharmacien |
| **PJ** | Templates Pipedrive ne supportent PAS les pièces jointes dynamiques |
| **Workaround PJ** | Lien vers SharePoint/Google Drive dans le template |
| **Note** | Email envoyé depuis le compte sync de l'utilisateur |

**Source** : [Pipedrive Email Workflow](https://www.pipedrive.com/en/features/email-workflow-automation)

---

### A9 - Alerte Validation Pharma > 10j
| Aspect | Évaluation |
|--------|------------|
| **Faisabilité** | ⚠️ 75% - Même limitation que A5 |
| **Workaround** | Créer activité "Suivi validation pharma" J+10 automatiquement |
| **Alternative** | Zapier avec check date entrée étape |

---

### A10 - Commande Auto Laphal
| Aspect | Évaluation |
|--------|------------|
| **Faisabilité** | ✅ 90% - Natif Pipedrive |
| **Trigger** | "Deal stage changed to Commande Échantillons" |
| **Action** | "Send email using template" à Laphal |
| **Contenu** | Variables : {organization.name}, {person.name}, etc. |
| **Note** | L'email part du compte commercial (signature incluse) |

---

### A11 - Création Activité Formation IDE
| Aspect | Évaluation |
|--------|------------|
| **Faisabilité** | ✅ 100% - Natif Pipedrive |
| **Trigger** | "Deal stage changed to Échantillons Livrés" |
| **Action** | "Create activity" type Tâche, échéance J+3 |
| **Assignation** | Au propriétaire du deal |

---

### A12 - Questionnaire Post-Formation IDE
| Aspect | Évaluation |
|--------|------------|
| **Faisabilité** | ✅ 90% - Natif Pipedrive |
| **Trigger** | "Deal field Formation IDE Pratique changed to Réalisée" |
| **Action** | "Send email" avec lien Fillout + créer activité suivi J+7 |
| **Personnalisation** | URL Fillout avec paramètres : `?deal_id={deal.id}&org={org.name}` |
| **Multi-destinataires** | 2 actions email séparées (IDE Référente + IDE Backup) |

---

### A13 - Questionnaires Post-Essais (3 destinataires)
| Aspect | Évaluation |
|--------|------------|
| **Faisabilité** | ✅ 85% - Natif Pipedrive |
| **Trigger** | "Deal field Statut Essais changed to Terminés" |
| **Actions** | 3 emails séquentiels avec templates différents |
| **Complexité** | 3 actions "Send email" + 1 "Create activity" |
| **Note** | Chaque email pointe vers le même formulaire Fillout avec `role=medecin/pharmacien/cadre` |

---

### A14 - Relance Questionnaires Non Répondus
| Aspect | Évaluation |
|--------|------------|
| **Faisabilité** | ⚠️ 70% - Complexe |
| **Problème** | Pipedrive ne sait pas si Fillout a reçu une réponse |
| **Solution** | A19 (webhook) doit d'abord marquer le deal "Réponse reçue" |
| **Alternative** | Relance systématique J+7 avec condition sur champ "Réponse Reçue = Non" |

---

### A16 - Vérification Doublons
| Aspect | Évaluation |
|--------|------------|
| **Faisabilité** | ⚠️ 60% - Limité nativement |
| **Natif** | Pipedrive détecte les doublons sur email/téléphone (merge suggéré) |
| **Limitation** | Pas de blocage automatique, juste suggestion visuelle |
| **Solution** | Accepter le comportement natif (suffisant pour la plupart des cas) |

---

### A17 - Champs Obligatoires Avant Progression ⭐
| Aspect | Évaluation |
|--------|------------|
| **Faisabilité** | ✅ 90% - Natif Pipedrive (Plan Pro) |
| **Configuration** | Settings → Data Fields → Required Fields |
| **Par étape** | Oui, configurable par pipeline ET par stage |
| **Limitation critique** | ⚠️ **Contournable via API, import, bulk-edit, automations** |
| **Impact** | Bloque uniquement les modifications manuelles UI |

**Source** : [Pipedrive Required Fields](https://support.pipedrive.com/en/article/required-fields)

> ⚠️ Cette limitation signifie que les automatisations (comme A7) peuvent créer des deals sans les champs requis.

---

### A18 - Historique Modifications
| Aspect | Évaluation |
|--------|------------|
| **Faisabilité** | ✅ 85% - Natif Pipedrive |
| **Trigger** | "Deal field [X] changed" |
| **Action** | "Create note" avec ancien/nouveau valeur |
| **Limitation** | Doit créer 1 automation par champ à surveiller |
| **Alternative** | Pipedrive garde déjà un historique dans l'onglet "History" du deal |

---

### A19 - Webhook Fillout → Pipedrive
| Aspect | Évaluation |
|--------|------------|
| **Faisabilité** | ✅ 95% - Via Zapier ou Fillout natif |
| **Option 1** | Fillout intégration native Pipedrive (2024) |
| **Option 2** | Zapier : Fillout New Submission → Pipedrive Update Deal |
| **Actions** | MAJ Score satisfaction, Créer note, Créer activité si score bas |

**Source** : [Fillout Pipedrive Integration](https://www.fillout.com/integrations/pipedrive)

---

## Synthèse par Niveau de Confiance

### ✅ Faisable Nativement (90-100%)
- A1 - Distribution leads
- A2 - Enrichissement contact
- A4 - Séquence relance ⭐⭐
- A6 - Notification équipe
- A7 - Création deal P2 ⭐
- A8 - Envoi dossier pharma ⭐
- A10 - Commande Laphal
- A11 - Activité formation IDE
- A12 - Questionnaire post-formation
- A13 - Questionnaires post-essais
- A17 - Champs obligatoires ⭐ (Plan Pro requis)
- A18 - Historique modifications

### ⚠️ Nécessite Workaround (70-85%)
- A3 - Normalisation organisation → Zapier Formatter
- A5 - Alerte stagnation ⭐⭐ → Activité auto ou DealFlow
- A9 - Alerte validation pharma → Activité auto
- A14 - Relance questionnaires → Dépend de A19
- A16 - Vérification doublons → Natif suffisant

### 🔌 Via Intégration Externe (Zapier/Make)
- A19 - Webhook Fillout → Pipedrive

---

## Recommandations Techniques

### 1. Plan Pipedrive
**Professional** minimum pour :
- Required Fields (A17)
- 60 automations actives (vs 30 sur Advanced)
- Revenue forecasting (bonus)

### 2. Outils Complémentaires
| Outil | Usage | Coût estimé |
|-------|-------|-------------|
| Zapier | A3, A19, backup A5/A9 | ~$20-50/mois |
| Fillout | Questionnaires | Gratuit-$29/mois |
| DealFlow (optionnel) | Alertes stagnation avancées | $? |

### 3. Workaround Alertes Stagnation (A5, A9)

Puisque le Rotting ne déclenche pas d'automation :

```
Trigger: Deal stage changed to [Étape]
Action 1: Create activity "Suivi stagnation"
          Due date: +7 jours (ou +10 pour A9)

--- Automation séparée ---

Trigger: Activity is due
Condition: Activity title contains "Suivi stagnation"
Action: Send notification email
```

### 4. Gestion Multi-Contacts (A12, A13)

Pour envoyer à plusieurs destinataires :
- Utiliser les champs Personne (Médecin Référent, Pharmacien, etc.)
- Créer 2-3 actions "Send email" dans la même automation
- Chaque action pointe vers un champ contact différent

---

## Points de Vigilance

### ⚠️ Limitation A17 (Required Fields)
Les champs obligatoires **ne bloquent PAS** les modifications via :
- API
- Import CSV
- Bulk edit
- Autres automatisations

**Impact** : L'automatisation A7 (création deal P2) pourrait créer un deal sans tous les champs remplis.

**Mitigation** : Ajouter dans A7 des valeurs par défaut pour les champs critiques.

### ⚠️ Emails depuis compte sync
Toutes les automatisations email envoient depuis le compte email synchronisé du propriétaire du deal.

**Impact** : Si le commercial n'a pas synchronisé son email, l'automation échoue.

**Mitigation** : Vérifier que tous les commerciaux ont leur email sync configuré.

### ⚠️ Limite de délai 12 semaines
Les automations Pipedrive ne peuvent pas avoir de délai > 84 jours.

**Impact** : OK pour tous nos cas (max = 10 jours pour A9).

---

## Sources

- [Pipedrive Workflow Automation](https://support.pipedrive.com/en/article/workflow-automation)
- [Pipedrive Required Fields](https://support.pipedrive.com/en/article/required-fields)
- [Pipedrive Rotting Feature](https://support.pipedrive.com/en/article/the-rotting-feature)
- [Pipedrive Automations Emails](https://support.pipedrive.com/en/article/automations-emails)
- [Pipedrive Automations Conditions](https://support.pipedrive.com/en/article/workflow-automation-conditions)
- [Fillout Pipedrive Integration](https://www.fillout.com/integrations/pipedrive)
- [Zapier Fillout + Pipedrive](https://zapier.com/apps/fillout/integrations/pipedrive)
- [Pipedrive Community - Deal Creation on Won](https://community.pipedrive.com/discussion/4707/create-deal-in-another-pipeline-automatically-when-marked-as-won)
- [DealFlow App](https://www.pipedrive.com/en/marketplace/app/deal-flow-notification-system/96a1b0d3be182e08)
