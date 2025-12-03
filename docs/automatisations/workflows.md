# Automatisations de Workflow

## A6 - Notification "Intéressé pour tester"

### Objectif
Alerter l'équipe quand un médecin accepte de tester, pour coordonner rapidement la suite.

### Déclencheur
- Deal Pipeline 1 passe à l'étape "Intéressé pour Tester"

### Actions
1. Notification au commercial propriétaire
2. Email au directeur médical avec récap du deal
3. Créer activité "Préparer dossier pharma"

### Template notification
```
🎉 Nouveau médecin intéressé pour tester !

Deal : {deal.title}
Établissement : {organization.name}
Service : {deal.specialite_service}
Médecin : {contact.name}

Prochaine étape : Préparer le dossier pharmaceutique
```

### Template email directeur médical
```
Objet : Nouveau médecin OK pour essais - {organization.name}

Bonjour,

Le Dr {contact.name} du service {deal.specialite_service} de
{organization.name} a accepté de tester Debrichem suite à la
formation théorique.

Informations deal :
- Commercial : {user.name}
- Source : {deal.source_lead}
- Date formation : {deal.date_formation}

La phase d'implémentation peut commencer.

Cordialement,
Système Pipedrive
```

### Niveau de confiance
**90% - Très probable** : Transition critique bien documentée

---

## A7 - Création automatique deal Pipeline 2

### Objectif
Assurer la continuité parfaite entre qualification et implémentation sans ressaisie.

### Déclencheur
- Deal Pipeline 1 "Qualification" marqué "Gagné"
- Étape finale : "Intéressé pour Tester"

### Actions

1. **Créer nouveau Deal dans Pipeline 2**
   - Pipeline : "Implémentation & Référencement"
   - Étape : "Validation Pharmaceutique"

2. **Copier les données**
   - Titre : identique
   - Valeur : identique
   - Organisation : liaison conservée
   - Contacts : tous les contacts liés copiés

3. **Copier les champs personnalisés**
   | Champ P1 | → | Champ P2 |
   |----------|---|----------|
   | Source Lead | → | Source Lead |
   | Spécialité Service | → | Spécialité Service |
   | Statut Formation Théorique | → | Statut Formation Théorique |

4. **Ajouter traçabilité**
   - Note : "Converti depuis deal #{deal_p1.id} le {date}"
   - Champ "Deal P1 Origine" = lien vers deal P1

5. **Notifications**
   - Notification au commercial "Deal converti vers Implémentation"
   - Créer activité "Identifier pharmacien"

### Configuration détaillée

```
Workflow Pipedrive :
├── Déclencheur : Deal marqué "Gagné" dans Pipeline 1
├── Condition : Étape = "Intéressé pour Tester"
└── Actions :
    ├── Créer Deal
    │   ├── Pipeline : Implémentation
    │   ├── Étape : Validation Pharmaceutique
    │   ├── Titre : {deal.title}
    │   ├── Organisation : {deal.org_id}
    │   └── Propriétaire : {deal.owner}
    ├── Copier champs (via API ou Make si nécessaire)
    ├── Lier contacts
    ├── Ajouter note
    └── Créer activité
```

### Gestion du deal P1 après conversion
- Le deal P1 reste marqué "Gagné"
- Visible dans l'historique et les statistiques
- Lien vers deal P2 pour traçabilité

### Protection anti-doublon
- Vérifier qu'un deal P2 n'existe pas déjà pour cette organisation/service
- Si doublon potentiel : notification au lieu de création auto

### Niveau de confiance
**80% - Très probable** : Dépend de la fiabilité du marquage "Gagné"

---

## A11 - Création activité formation IDE

### Objectif
Déclencher automatiquement l'organisation de la formation dès que les échantillons sont disponibles.

### Déclencheur
- Champ "Statut Échantillons" passe à "Livrés"

### Actions

1. **Créer activité**
   - Type : Appel
   - Titre : "Contacter cadre pour planifier formation IDE"
   - Échéance : J+2
   - Assigné : Commercial propriétaire du deal

2. **Ajouter note checklist**
```
═══════════════════════════════════════
CHECKLIST FORMATION IDE - {deal.title}
═══════════════════════════════════════

Produits disponibles en pharmacie ✓

À organiser :
[ ] Date formation confirmée
[ ] Lieu identifié (service)
[ ] IDE référente identifiée
[ ] IDE backup identifiée
[ ] Matériel formation préparé
[ ] Protocole à transmettre

Contacts à joindre :
- Cadre de santé : {contact_cadre.name} - {contact_cadre.phone}
- Pharmacien : {contact_pharma.name} - {contact_pharma.phone}
```

3. **Notification**
   - "Échantillons livrés - organiser formation IDE"

### Configuration
```
Déclencheur : Champ mis à jour
├── Champ : Statut Échantillons
├── Nouvelle valeur : Livrés
└── Actions :
    ├── Créer activité (Appel, J+2)
    ├── Ajouter note (checklist)
    └── Notification
```

### Niveau de confiance
**90% - Très probable** : Action simple et bien définie

---

## Schéma des workflows

```
PIPELINE 1                              PIPELINE 2
─────────────────────────────────────────────────────────────────

                                     ┌─────────────────────┐
                                     │ VALIDATION PHARMA   │
                                     │                     │
                                     │ • Dossier envoyé    │
  ┌──────────────────┐               │   (A8)              │
  │ INTÉRESSÉ POUR   │───────A7──────│ • Alerte 10j (A9)   │
  │ TESTER           │               │                     │
  │                  │               └──────────┬──────────┘
  │ • Notif équipe   │                          │
  │   (A6)           │                          ▼
  └──────────────────┘               ┌─────────────────────┐
                                     │ COMMANDE ÉCHANT.    │
                                     │                     │
                                     │ • Mail Laphal (A10) │
                                     │                     │
                                     └──────────┬──────────┘
                                                │
                                                ▼
                                     ┌─────────────────────┐
                                     │ ÉCHANTILLONS LIVRÉS │
                                     │                     │
                                     │ • Activité formation│
                                     │   (A11)             │
                                     └─────────────────────┘
```

---

## Points d'attention

### A7 - Conversion P1 → P2
- Tester sur un deal réel avant déploiement général
- Vérifier que tous les contacts sont bien copiés
- S'assurer que les champs personnalisés passent correctement

### A11 - Activité formation
- Le cadre de santé doit être identifié dans le deal
- Si pas de cadre → créer activité "Identifier cadre de santé" en priorité

### Maintenance
- Vérifier régulièrement que les workflows fonctionnent
- Monitorer les échecs via A18 (si déployé)
