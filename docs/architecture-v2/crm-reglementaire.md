# CRM Réglementaire - Spécifications

**Objectif** : Assurer la conformité réglementaire dans les interactions avec les professionnels de santé

---

## Principes directeurs

1. **Simplicité** : Uniquement les champs obligatoires/nécessaires
2. **Conformité** : Traçabilité complète pour les audits
3. **Efficacité** : Saisie rapide par les commerciaux

---

## Entités

### 1. Personnes (Contacts)

#### Champs d'identification

| Champ | Type | Obligatoire | Notes |
|-------|------|-------------|-------|
| Nom | Texte | Oui | - |
| Prénom | Texte | Oui | - |
| Email | Email | Recommandé | Pour communications |
| Téléphone | Téléphone | Recommandé | - |
| RPPS | Texte | Recommandé | Identifiant national |
| Établissement | Lien Organisation | Oui | Lien vers fiche établissement |
| Service | Texte | Oui | Ex: Néphrologie, Réanimation |

#### Champs de qualification

| Champ | Type | Options | Logique |
|-------|------|---------|---------|
| Qualification | Dropdown | Utilisateur régulier / Décideur / KOL | Détermine fréquence visite |
| Rôle | Dropdown | Médecin / Pharmacien / Cadre / IDE | Classification métier |
| Spécialité | Texte | Libre | Ex: Néphrologue |

**Règles de fréquence de visite** :

| Qualification | Fréquence minimum | Alerte si dépassé |
|--------------|-------------------|-------------------|
| Utilisateur régulier | 1x / mois | +7 jours |
| Décideur | 1x / trimestre | +14 jours |
| KOL | 1x / 6 mois | +30 jours |

> Si une personne cumule plusieurs qualifications, la fréquence la plus élevée s'applique.

#### Champs réglementaires

| Champ | Type | Options | Usage |
|-------|------|---------|-------|
| E32 Consentement | Dropdown | Non demandé / En attente / Oui / Non | Consentement contact individuel |
| E32 Date | Date | - | Date d'obtention |
| E34 Politique | Dropdown | Non / Oui | Politique établissement |
| E35 | Dropdown | Selon besoin | Autre conformité |

#### Champs réseau

| Champ | Type | Notes |
|-------|------|-------|
| Réseau | Multi-lien Personne | Autres contacts liés |
| Société savante | Texte | Appartenance associations |
| Études cliniques | Fichiers | PDFs études publiées |
| Étiologie | Tags | Spécialités pathologies |

---

### 2. Organisations (Établissements)

| Champ | Type | Options | Notes |
|-------|------|---------|-------|
| Nom | Texte | - | Nom officiel |
| Type | Dropdown | CHU / CH / Clinique / EHPAD / Autre | - |
| Groupe | Texte | - | Ex: APHP, APHM, HCL |
| Région | Dropdown | Régions France | Pour assignation |
| Adresse | Texte | - | - |
| Nombre de lits | Nombre | - | Potentiel volume |
| Code FINESS | Texte | - | Identifiant officiel |

---

### 3. Activités

#### Types d'activités

| Type | Icône | Contraintes | Notes |
|------|-------|-------------|-------|
| Repas | 🍽️ | Max 2/an/personne | Alerte automatique |
| Face-face | 👤 | - | Visite présentielle |
| Vidéo | 📹 | - | Teams, Zoom, etc. |
| Téléphone | 📞 | - | Appel téléphonique |
| Congrès | 🎤 | - | Événement professionnel |
| Webinar | 💻 | - | Formation en ligne |

#### Champs par activité

| Champ | Type | Notes |
|-------|------|-------|
| Type | Dropdown | Voir liste ci-dessus |
| Date | Date | - |
| Créneau | Dropdown | Matin / Après-midi | Réglementaire |
| Durée | Nombre | En minutes |
| Personne | Lien | Contact concerné |
| Notes | Texte | Compte-rendu |
| Projet lié | Lien | Optionnel, lien vers projet établissement |

#### Nommage des activités

Format : `[Type] - [Personne] - [Détail optionnel]`

Exemples :
- `Repas - Dr Langeron - Restaurant Le Petit`
- `Face-face - Dr Aubrain`
- `Vidéo - Équipe Plaies HEGP`
- `Congrès - SFAR 2024`

---

## Tableaux de bord CRM

### 1. Couverture des cibles

**Objectif** : Vérifier que les contacts sont vus selon leur fréquence

| Métrique | Calcul |
|----------|--------|
| Contacts à jour | Nb contacts vus dans les délais / Nb total |
| Contacts en retard | Liste des contacts non vus dans les délais |
| Prochaines visites | Contacts à voir dans les 7 prochains jours |

### 2. Activité commerciale

| Métrique | Calcul |
|----------|--------|
| Activités / commercial | Nb activités par personne sur période |
| Répartition par type | % de chaque type d'activité |
| Temps passé | Somme des durées par établissement |

### 3. Conformité repas

| Métrique | Calcul |
|----------|--------|
| Repas par contact | Nb repas YTD par personne |
| Alertes repas | Contacts à 2 repas (limite atteinte) |
| Export transparence | Données pour déclaration |

---

## Automatisations

### A-CRM-1 : Alerte limite repas

**Déclencheur** : Création activité "Repas"
**Condition** : Contact a déjà 1 repas dans l'année
**Action** : Notification "Attention : dernier repas autorisé pour [Nom]"

### A-CRM-2 : Alerte retard visite

**Déclencheur** : Quotidien (batch)
**Condition** : Délai depuis dernière activité > seuil selon qualification
**Action** : Email récapitulatif des contacts en retard

### A-CRM-3 : Export transparence

**Déclencheur** : 1er du mois
**Condition** : Activités "Repas" du mois précédent
**Action** : Génération CSV pour déclaration transparence

---

## Migration depuis v1

### Champs à conserver
- Tous les contacts existants
- Historique des activités
- Organisations

### Champs à supprimer/archiver
- ~~Pipeline de qualification~~
- ~~Étapes commerciales~~
- ~~Statuts de formation~~

### Champs à ajouter
- Qualification (Utilisateur régulier / Décideur / KOL)
- E32, E34, E35
- Réseau
- Société savante
