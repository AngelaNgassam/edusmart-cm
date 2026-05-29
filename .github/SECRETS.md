# GitHub Secrets — Configuration requise

Ajoute ces secrets dans **Settings → Secrets and variables → Actions** de ton repo GitHub.

---

## 🐳 Docker Hub (obligatoires)

| Secret | Description | Exemple |
|--------|-------------|---------|
| `DOCKERHUB_USERNAME` | Ton username Docker Hub | `monusername` |
| `DOCKER_PASSWORD` | Ton mot de passe ou Access Token Docker Hub | `dckr_pat_xxx...` |

> **Recommandé** : utilise un **Access Token** Docker Hub plutôt que ton mot de passe.
> Crée-le sur https://hub.docker.com → Account Settings → Security → New Access Token

---

## 🌐 Environnement Staging (`staging` branch)

### Serveur
| Secret | Description |
|--------|-------------|
| `STAGING_SSH_HOST` | IP ou domaine du serveur |
| `STAGING_SSH_USER` | Utilisateur SSH (ex: `ubuntu`, `deploy`) |
| `STAGING_SSH_KEY` | Clé privée SSH complète (contenu du fichier) |
| `STAGING_SSH_PORT` | Port SSH (laisser vide = 22) |
| `STAGING_APP_PATH` | Chemin du projet sur le serveur (ex: `/home/ubuntu/edusmart`) |

### Application
| Secret | Description |
|--------|-------------|
| `STAGING_APP_KEY` | Laravel APP_KEY (généré avec `php artisan key:generate --show`) |
| `STAGING_APP_URL` | URL backend (ex: `https://api-staging.edusmart.cm`) |
| `STAGING_FRONTEND_URL` | URL frontend (ex: `https://staging.edusmart.cm`) |
| `STAGING_VITE_API_URL` | URL API pour le frontend (ex: `https://api-staging.edusmart.cm/api`) |

### Base de données
| Secret | Description |
|--------|-------------|
| `STAGING_DB_DATABASE` | Nom de la base (ex: `edusmartcm_staging`) |
| `STAGING_DB_USERNAME` | Utilisateur MySQL |
| `STAGING_DB_PASSWORD` | Mot de passe MySQL |
| `STAGING_DB_ROOT_PASSWORD` | Mot de passe root MySQL |

---

## 🚀 Environnement Production (`main` branch)

### Serveur
| Secret | Description |
|--------|-------------|
| `PROD_SSH_HOST` | IP ou domaine du serveur |
| `PROD_SSH_USER` | Utilisateur SSH |
| `PROD_SSH_KEY` | Clé privée SSH complète |
| `PROD_SSH_PORT` | Port SSH (laisser vide = 22) |
| `PROD_APP_PATH` | Chemin du projet (ex: `/home/ubuntu/edusmart`) |

### Application
| Secret | Description |
|--------|-------------|
| `PROD_APP_KEY` | Laravel APP_KEY |
| `PROD_APP_URL` | URL backend (ex: `https://api.edusmart.cm`) |
| `PROD_FRONTEND_URL` | URL frontend (ex: `https://edusmart.cm`) |
| `PROD_VITE_API_URL` | URL API pour le frontend (ex: `https://api.edusmart.cm/api`) |

### Base de données
| Secret | Description |
|--------|-------------|
| `PROD_DB_DATABASE` | Nom de la base (ex: `edusmartcm_db`) |
| `PROD_DB_USERNAME` | Utilisateur MySQL |
| `PROD_DB_PASSWORD` | Mot de passe MySQL |
| `PROD_DB_ROOT_PASSWORD` | Mot de passe root MySQL |

---

## 🔑 Générer une clé SSH pour le déploiement

```bash
# Sur ta machine locale
ssh-keygen -t ed25519 -C "github-actions-deploy" -f ~/.ssh/edusmart_deploy

# Copier la clé publique sur le serveur
ssh-copy-id -i ~/.ssh/edusmart_deploy.pub user@ton-serveur

# Afficher la clé privée à coller dans le secret GitHub
cat ~/.ssh/edusmart_deploy
```

## 📦 Images Docker Hub créées automatiquement

- `DOCKER_USERNAME/edusmart-backend:latest` → production
- `DOCKER_USERNAME/edusmart-backend:staging` → staging
- `DOCKER_USERNAME/edusmart-frontend:latest` → production
- `DOCKER_USERNAME/edusmart-frontend:staging` → staging
