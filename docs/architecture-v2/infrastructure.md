# Infrastructure Technique - Spécifications

**Objectif** : Héberger les outils complémentaires à Pipedrive (automatisations, base de données, interface)

---

## Vue d'ensemble

```
┌─────────────────────────────────────────────────────────────────────┐
│                    SERVEUR HOSTINGER VPS                            │
│                    Ubuntu 24.04 LTS                                 │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│   ┌─────────────────┐   ┌─────────────────┐   ┌─────────────────┐   │
│   │      N8N        │   │   PostgreSQL    │   │   Interface     │   │
│   │                 │   │                 │   │   Web           │   │
│   │  Automatisations│   │  Base soignants │   │  Recherche      │   │
│   │  Workflows      │   │  + Données Debex│   │  + Import CRM   │   │
│   │                 │   │                 │   │                 │   │
│   └────────┬────────┘   └────────┬────────┘   └────────┬────────┘   │
│            │                     │                     │            │
│            └─────────────────────┼─────────────────────┘            │
│                                  │                                  │
│                           ┌──────▼──────┐                           │
│                           │   Nginx     │                           │
│                           │   Reverse   │                           │
│                           │   Proxy     │                           │
│                           └──────┬──────┘                           │
│                                  │                                  │
└──────────────────────────────────┼──────────────────────────────────┘
                                   │
                            ┌──────▼──────┐
                            │   Internet  │
                            │             │
                            │ api.debx.fr │
                            │ n8n.debx.fr │
                            │ app.debx.fr │
                            └─────────────┘
```

---

## Serveur Hostinger

### Spécifications

| Caractéristique | Valeur |
|-----------------|--------|
| Fournisseur | Hostinger |
| Type | VPS |
| Plan | KVM 1 (ou équivalent) |
| OS | Ubuntu 24.04 LTS |
| RAM | 4 GB |
| CPU | 2 vCPU |
| Stockage | 50 GB NVMe |
| Bande passante | 4 TB |
| Coût | ~10€/mois |

### Accès

- SSH : Clé privée (pas de mot de passe)
- Utilisateur : debx (sudo)
- IP : (à documenter après installation)

---

## PostgreSQL

### Usage

Base de données principale contenant :
1. **Base soignants** : Importée depuis Supabase
2. **Données Debex** : Compléments métier

### Structure base soignants

```sql
-- Table principale des soignants
CREATE TABLE soignants (
    id SERIAL PRIMARY KEY,
    rpps VARCHAR(11) UNIQUE,
    nom VARCHAR(100),
    prenom VARCHAR(100),
    email VARCHAR(255),
    telephone VARCHAR(20),
    specialite VARCHAR(100),
    etablissement_id INTEGER REFERENCES etablissements(id),
    service VARCHAR(100),
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);

-- Table des établissements
CREATE TABLE etablissements (
    id SERIAL PRIMARY KEY,
    nom VARCHAR(200),
    type VARCHAR(50), -- CHU, CH, Clinique, EHPAD
    groupe VARCHAR(100), -- APHP, APHM, etc.
    region VARCHAR(100),
    code_finess VARCHAR(20),
    adresse TEXT,
    nombre_lits INTEGER
);

-- Index pour recherche rapide
CREATE INDEX idx_soignants_nom ON soignants(nom);
CREATE INDEX idx_soignants_rpps ON soignants(rpps);
CREATE INDEX idx_soignants_etablissement ON soignants(etablissement_id);
```

### Migration depuis Supabase

```bash
# Export depuis Supabase
pg_dump -h db.xxxx.supabase.co -U postgres -d postgres > backup.sql

# Import sur Hostinger
psql -h localhost -U debx -d debx < backup.sql
```

---

## N8N

### Installation

```bash
# Installation via Docker
docker run -d \
  --name n8n \
  -p 5678:5678 \
  -e N8N_BASIC_AUTH_ACTIVE=true \
  -e N8N_BASIC_AUTH_USER=admin \
  -e N8N_BASIC_AUTH_PASSWORD=<password> \
  -v n8n_data:/home/node/.n8n \
  n8nio/n8n
```

### Configuration

| Paramètre | Valeur |
|-----------|--------|
| URL | https://n8n.debx.fr |
| Auth | Basic Auth |
| Timezone | Europe/Paris |

### Workflows à créer

#### W1 : Alerte repas > 2/an

```
Déclencheur: Webhook Pipedrive (nouvelle activité)
    ↓
Condition: Type = "Repas"
    ↓
Query PostgreSQL: Compter repas pour ce contact cette année
    ↓
Si count >= 2:
    ↓
Envoyer notification (email ou Slack)
```

#### W2 : Alerte retard visite

```
Déclencheur: Cron quotidien (8h)
    ↓
Query Pipedrive: Contacts avec qualification
    ↓
Pour chaque contact:
    Query: Dernière activité
    Calculer: Délai vs fréquence attendue
    ↓
Si dépassé:
    Ajouter à liste alertes
    ↓
Envoyer email récapitulatif
```

#### W3 : Export transparence mensuel

```
Déclencheur: Cron 1er du mois (6h)
    ↓
Query Pipedrive: Activités "Repas" du mois précédent
    ↓
Formater données pour déclaration
    ↓
Générer CSV
    ↓
Envoyer par email + sauvegarder sur Drive
```

#### W4 : Import soignant vers CRM

```
Déclencheur: Webhook depuis interface web
    ↓
Données: soignant_id, action
    ↓
Query PostgreSQL: Récupérer données soignant
    ↓
API Pipedrive: Créer/Mettre à jour contact
    ↓
Réponse: Succès ou erreur
```

---

## Interface Web

### Objectif

Application web simple permettant de :
1. Rechercher dans la base des soignants
2. Ajouter des soignants au CRM Pipedrive en 1 clic
3. Visualiser les soignants déjà dans le CRM

### Stack technique

| Composant | Technologie |
|-----------|-------------|
| Frontend | React ou Vue.js |
| Backend | Node.js + Express |
| ORM | Prisma |
| Auth | OAuth Microsoft (SSO) |

### Fonctionnalités

#### Recherche

```
┌─────────────────────────────────────────────────────┐
│  Recherche soignants                                │
├─────────────────────────────────────────────────────┤
│                                                     │
│  Nom: [_______________]  Prénom: [_______________]  │
│                                                     │
│  Établissement: [▼ Tous            ]                │
│  Spécialité:    [▼ Toutes          ]                │
│                                                     │
│  [🔍 Rechercher]                                    │
│                                                     │
├─────────────────────────────────────────────────────┤
│  Résultats (127 trouvés)                            │
├─────────────────────────────────────────────────────┤
│  ☐ Dr Jean DUPONT - Néphrologue - CHU Lyon         │
│    📧 j.dupont@chu-lyon.fr | 📞 04 72 xx xx xx     │
│    [✅ Dans CRM] [👁️ Voir fiche]                   │
│                                                     │
│  ☐ Dr Marie MARTIN - Réanimatrice - APHP Pitié     │
│    📧 m.martin@aphp.fr | 📞 01 42 xx xx xx         │
│    [➕ Ajouter au CRM]                              │
│                                                     │
└─────────────────────────────────────────────────────┘
```

#### Import CRM

1. Utilisateur sélectionne un soignant
2. Clic sur "Ajouter au CRM"
3. Appel API N8N (workflow W4)
4. Contact créé dans Pipedrive
5. Feedback visuel (succès/erreur)

### Déploiement

```bash
# Build
npm run build

# Déploiement via PM2
pm2 start npm --name "debx-app" -- start

# Ou via Docker
docker build -t debx-app .
docker run -d -p 3000:3000 debx-app
```

---

## Nginx Reverse Proxy

### Configuration

```nginx
# /etc/nginx/sites-available/debx

server {
    listen 80;
    server_name n8n.debx.fr app.debx.fr api.debx.fr;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl;
    server_name n8n.debx.fr;

    ssl_certificate /etc/letsencrypt/live/debx.fr/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/debx.fr/privkey.pem;

    location / {
        proxy_pass http://localhost:5678;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
    }
}

server {
    listen 443 ssl;
    server_name app.debx.fr;

    ssl_certificate /etc/letsencrypt/live/debx.fr/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/debx.fr/privkey.pem;

    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

### SSL avec Let's Encrypt

```bash
# Installation Certbot
apt install certbot python3-certbot-nginx

# Génération certificat
certbot --nginx -d n8n.debx.fr -d app.debx.fr -d api.debx.fr

# Renouvellement automatique
certbot renew --dry-run
```

---

## Sécurité

### Firewall (UFW)

```bash
ufw allow ssh
ufw allow http
ufw allow https
ufw enable
```

### Accès base de données

- PostgreSQL écoute uniquement sur localhost
- Pas d'accès externe direct
- Connexion via application uniquement

### Sauvegardes

```bash
# Backup quotidien PostgreSQL
0 3 * * * pg_dump -U debx debx | gzip > /backup/debx_$(date +\%Y\%m\%d).sql.gz

# Retention 30 jours
find /backup -name "*.sql.gz" -mtime +30 -delete
```

---

## Planning installation

### Jour 1 : Serveur de base
- [ ] Accès SSH configuré
- [ ] Utilisateur debx créé
- [ ] UFW activé
- [ ] Docker installé

### Jour 2 : Services
- [ ] PostgreSQL installé
- [ ] N8N déployé
- [ ] Nginx configuré
- [ ] SSL activé

### Jour 3 : Données
- [ ] Migration Supabase → PostgreSQL
- [ ] Vérification intégrité données
- [ ] Test connexion N8N ↔ PostgreSQL

### Jour 4 : Interface
- [ ] Développement interface recherche
- [ ] Intégration API Pipedrive
- [ ] Tests end-to-end

### Jour 5 : Production
- [ ] DNS configurés
- [ ] Tests finaux
- [ ] Documentation utilisateur
- [ ] Formation équipe
