# EDUSMART-CM

Projet académique - Module [Administration - Enseignant]

3 collaborateurs :
NGASSAM Angela
Metangmo Bersaine
Takam Manuel

## Structure
- `backend/` — API Laravel
- `frontend/` — React Vite

## Installation

### Avec Docker (recommandé)

1) Copier le fichier d'environnement pour `docker-compose`:

```bash
cp .env.example .env
```

2) Générer la clé Laravel et la coller dans `.env` (variable `APP_KEY`):

```bash
cd backend
php artisan key:generate --show
```

3) Lancer les conteneurs:

```bash
cd ..
docker compose up -d --build
```

4) Lancer les migrations (premier démarrage):

```bash
docker compose exec -T backend php artisan migrate --force
```

- Backend: `http://localhost:8000`
- Frontend: `http://localhost:3000`

## Déploiement automatique vers Docker Hub

À chaque push sur `main` ou `staging`, GitHub Actions build et publie :

- `VOTRE_USER/edusmart-backend:latest` (ou `:staging`)
- `VOTRE_USER/edusmart-frontend:latest` (ou `:staging`)

### 1) Créer un token Docker Hub

1. Compte sur [hub.docker.com](https://hub.docker.com)
2. **Account Settings → Security → New Access Token** (droits **Read & Write**)
3. Copier le token (affiché une seule fois)

### 2) Ajouter les secrets GitHub

Repo → **Settings → Secrets and variables → Actions → New repository secret** :

| Secret | Valeur |
|--------|--------|
| `DOCKERHUB_USERNAME` | votre identifiant Docker Hub |
| `DOCKER_PASSWORD` | le token (pas le mot de passe du compte) |

Optionnel (URL API pour le build frontend) :

| Secret | Exemple |
|--------|---------|
| `PROD_VITE_API_URL` | `https://api.mondomaine.com/api` |
| `STAGING_VITE_API_URL` | `https://staging-api.mondomaine.com/api` |

### 3) Pousser sur `main`

```bash
git add .
git commit -m "Configure Docker Hub publish"
git push origin main
```

Vérifier l’onglet **Actions** → workflow **Docker Hub — Build & Push**.

### 4) Tirer les images sur un serveur (manuel)

```bash
docker login
docker pull VOTRE_USER/edusmart-backend:latest
docker pull VOTRE_USER/edusmart-frontend:latest

# avec docker-compose.prod.yml
export BACKEND_IMAGE=VOTRE_USER/edusmart-backend:latest
export FRONTEND_IMAGE=VOTRE_USER/edusmart-frontend:latest
# + APP_KEY, DB_PASSWORD, etc. dans .env
docker compose -f docker-compose.prod.yml up -d
```

### Déploiement SSH automatique (plus tard)

Par défaut, seul Docker Hub est activé. Pour déployer sur un VPS après le push :

1. Variable **Actions → Variables** : `ENABLE_SSH_DEPLOY` = `true`
2. Secrets SSH : `PROD_SSH_HOST`, `PROD_SSH_USER`, `PROD_SSH_KEY`, `PROD_APP_PATH`, etc.

### Sans Docker

#### Backend

```bash
cd backend
cp .env.example .env
composer install
php artisan key:generate
php artisan migrate
php artisan serve
```

#### Frontend

```bash
cd frontend
npm install
npm run dev
```

