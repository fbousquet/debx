# Roadmap Déploiement CRM Pipedrive - Debex-Médical

## Vue d'ensemble

| Semaine | Focus | Objectif |
|---------|-------|----------|
| S1 | Fondations | Pipelines, champs, structure de base |
| S2 | Quick Wins | Automatisations de relance et alertes |
| S3 | Documents | Workflows validation pharma et commandes |
| S4 | Continuité | Questionnaires et suivi satisfaction |
| Buffer | Ajustements | Formation équipe, corrections, monitoring |

## Prérequis

- [ ] Accès admin Pipedrive
- [ ] Liste des 3 commerciaux avec emails
- [ ] Email du directeur médical
- [ ] Templates documents Word existants (dossier pharma, devis)
- [ ] Compte Fillout configuré
- [ ] Mapping territoires commerciaux défini

## Dépendances critiques

```
S1 Fondations
    │
    ├── Pipelines créés ──────────────────┐
    │                                      │
    ├── Champs personnalisés ─────────────┼──► S2-S4 Automatisations
    │                                      │
    └── Contacts importés avec rôles ─────┘
```

## Jalons clés

| Jalon | Description | Semaine |
|-------|-------------|---------|
| M1 | Pipelines opérationnels | Fin S1 |
| M2 | Premiers deals créés | S1-S2 |
| M3 | Relances automatiques actives | Fin S2 |
| M4 | Workflow pharma fonctionnel | Fin S3 |
| M5 | Parcours complet testé end-to-end | Fin S4 |
| M6 | Équipe formée et autonome | Buffer |

## Risques identifiés

| Risque | Impact | Mitigation |
|--------|--------|------------|
| Commerciaux ne remplissent pas les champs | Automatisations cassées | Champs obligatoires (A17) |
| Doublons contacts | Confusion, mauvaise communication | Détection doublons (A16) |
| Templates email mal formatés | Envois échoués | Tests avant mise en prod |
| Intégration Fillout complexe | Retard S4 | Prévoir fallback manuel |

## Métriques de succès (qualitatives)

- [ ] Aucun deal "zombie" (sans activité >14j)
- [ ] Tous les deals ont un propriétaire assigné
- [ ] Chaque deal a les interlocuteurs clés identifiés
- [ ] Transitions entre pipelines fluides
- [ ] Commerciaux utilisent Pipedrive quotidiennement

## Contacts projet

| Rôle | Responsabilité |
|------|----------------|
| DSI (vous) | Configuration, automatisations, formation |
| Directeur médical Debex | Validation process, RDV médecins |
| Commerciaux (3) | Utilisateurs finaux, feedback terrain |
