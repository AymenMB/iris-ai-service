# TP2 : Docker et Docker Compose pour MLOps
# Projet : API FastAPI + Frontend React

## 👨‍🎓 Informations du Projet

**Étudiant:** Aymen Mabrouk
**Date:** 30 Octobre 2025  
**Cours:** MLOps 2025-26 - Dr. Salah Gontara  
**Dépôt GitLab:** https://gitlab.com/AymenMB1/iris-ai-service.git

---

## 🎯 Objectif

Containeriser et orchestrer un service IA complet composé de :
- **API FastAPI** : Serveur backend exposant un modèle de prédiction Iris (RandomForest)
- **Frontend React** : Interface utilisateur pour interagir avec l'API
- **Monitoring** : Prometheus et Grafana pour la surveillance des métriques

---

## 🏗️ Architecture du Projet

```
iris-ai-service/
├── api/
│   ├── app/
│   │   ├── main.py          # Point d'entrée FastAPI
│   │   ├── models.py        # Modèles Pydantic
│   │   ├── db.py            # Gestion du modèle ML
│   │   └── model/           # Dossier contenant model.joblib
│   ├── requirements.txt     # Dépendances Python
│   └── Dockerfile          # ✅ Créé
├── frontend/
│   ├── src/                 # Code source React
│   ├── package.json         # Dépendances Node.js
│   ├── nginx.conf           # Configuration nginx
│   └── Dockerfile          # ✅ Créé
└── docker-compose.yml      # ✅ Créé
```

---

## 📋 Prérequis

- Docker Desktop installé et démarré
- Git installé
- Ports 8000 et 5174 disponibles

---

## 🚀 Instructions d'Installation et d'Exécution

### 1️⃣ Cloner le Projet

```bash
git clone https://gitlab.com/AymenMB1/iris-ai-service.git
cd iris-ai-service
```

### 2️⃣ Construire et Tester l'API Individuellement

```bash
# Se déplacer dans le dossier API
cd api

# Construire l'image Docker
docker build -t iris-api:dev .

# Lancer le conteneur
docker run -d -p 8000:8000 --name iris-api iris-api:dev

# Tester l'API
curl http://localhost:8000/health

# Arrêter et supprimer le conteneur
docker stop iris-api
docker rm iris-api
```

**Points d'accès de l'API:**
- 🏥 Healthcheck : http://localhost:8000/health
- 📚 Documentation Swagger : http://localhost:8000/docs
- 🔮 Endpoint de prédiction : http://localhost:8000/predict

### 3️⃣ Construire et Tester le Frontend Individuellement

```bash
# Retour à la racine puis aller dans frontend
cd ../frontend

# Construire l'image Docker
docker build -t iris-frontend:dev .

# Lancer le conteneur
docker run -d -p 5174:80 --name iris-frontend iris-frontend:dev

# Arrêter et supprimer le conteneur
docker stop iris-frontend
docker rm iris-frontend
```

**Point d'accès du Frontend:**
- 🌐 Interface Web : http://localhost:5174

### 4️⃣ Lancer l'Application Complète avec Docker Compose

```bash
# Retour à la racine du projet
cd ..

# Construire et lancer tous les services
docker compose up --build

# Ou en mode détaché (arrière-plan)
docker compose up --build -d

# Voir les logs
docker compose logs -f

# Arrêter tous les services
docker compose down
```

---

## 🔍 Détails des Fichiers Créés

### 📄 `api/Dockerfile`

**Description:** Containerise l'API FastAPI

**Caractéristiques:**
- Image de base : `python:3.11-slim`
- Installation des dépendances depuis `requirements.txt`
- Copie du code applicatif (`app/`)
- Exposition du port `8000`
- Healthcheck intégré
- Commande : `uvicorn app.main:app --host 0.0.0.0 --port 8000`

### 📄 `frontend/Dockerfile`

**Description:** Multi-stage build pour le frontend React

**Stage 1 - Builder:**
- Image : `node:20-alpine`
- Installation des dépendances npm
- Build de production avec Vite

**Stage 2 - Production:**
- Image : `nginx:alpine`
- Configuration nginx personnalisée
- Copie des fichiers buildés
- Exposition du port `80`
- Healthcheck intégré

### 📄 `docker-compose.yml`

**Description:** Orchestration des services

**Services:**

**1. API Service:**
- Build depuis `./api`
- Port : `8000:8000`
- Variables d'environnement configurées
- Healthcheck actif
- Réseau : `iris-network`
- Métriques Prometheus exposées sur `/metrics`

**2. Frontend Service:**
- Build depuis `./frontend`
- Port : `5174:80`
- Dépend du service API (avec healthcheck)
- Variables d'environnement pour la connexion API
- Réseau : `iris-network`

**3. Prometheus Service:**
- Image : `prom/prometheus:latest`
- Port : `9090:9090`
- Configuration depuis `./monitoring/prometheus.yml`
- Collecte les métriques de l'API sur `api:8000/metrics`
- Scrape interval : 5 secondes

**4. Grafana Service:**
- Image : `grafana/grafana:latest`
- Port : `3000:3000`
- Identifiants : `admin` / `admin`
- Datasource Prometheus pré-configurée
- Dashboard FastAPI pré-importé
- Visualisation des requêtes, latence et erreurs

**Variables d'Environnement:**
```
API_PORT=8000
CORS_ORIGINS=http://localhost:5174
VITE_API_BASE=http://localhost:8000
```

---

## 🧪 Tests et Validation

### Test de l'API

```bash
# Healthcheck
curl http://localhost:8000/health

# Prédiction (exemple)
curl -X POST http://localhost:8000/predict \
  -H "Content-Type: application/json" \
  -d '{
    "sepal_length": 5.1,
    "sepal_width": 3.5,
    "petal_length": 1.4,
    "petal_width": 0.2
  }'
```

### Test du Frontend

Ouvrir le navigateur à l'adresse : http://localhost:5174

Vérifier que :
- L'interface se charge correctement
- Les champs de saisie sont présents
- La connexion à l'API fonctionne

### Test de Prometheus

Ouvrir le navigateur à l'adresse : http://localhost:9090

Vérifier que :
- L'interface Prometheus se charge
- L'API est listée dans les targets (Status > Targets)
- Les métriques sont collectées

### Test de Grafana

1. Ouvrir le navigateur à l'adresse : http://localhost:3000
2. Se connecter avec : `admin` / `admin`
3. Vérifier que :
   - La datasource Prometheus est configurée
   - Le dashboard FastAPI est disponible
   - Les métriques s'affichent (requêtes, latence, erreurs)

---

## 📊 Commandes Docker Utiles

```bash
# Voir les conteneurs en cours d'exécution
docker ps

# Voir toutes les images
docker images

# Voir les logs d'un service spécifique
docker compose logs api
docker compose logs frontend

# Redémarrer un service
docker compose restart api

# Reconstruire sans cache
docker compose build --no-cache

# Nettoyer les ressources
docker compose down -v --rmi all
```

---

## 🐛 Dépannage

### Le port 8000 est déjà utilisé
```bash
# Windows PowerShell
netstat -ano | findstr :8000
# Puis tuer le processus avec son PID
taskkill /PID <PID> /F
```

### Le port 5174 est déjà utilisé
```bash
# Windows PowerShell
netstat -ano | findstr :5174
# Puis tuer le processus avec son PID
taskkill /PID <PID> /F
```

### Reconstruire complètement
```bash
docker compose down -v
docker compose build --no-cache
docker compose up
```

---

## 📚 Technologies Utilisées

- **Backend:** FastAPI 0.115.0, Uvicorn, scikit-learn
- **Frontend:** React 18, Vite 5
- **ML:** RandomForest (Iris dataset)
- **Monitoring:** Prometheus (latest), Grafana (latest)
- **Instrumentation:** prometheus-fastapi-instrumentator 6.1.0
- **Containerisation:** Docker, Docker Compose
- **Web Server:** Nginx (Alpine)

---

## ✅ Livrables

- ✅ `api/Dockerfile` - Image Python pour l'API FastAPI
- ✅ `frontend/Dockerfile` - Multi-stage build pour React + Nginx
- ✅ `docker-compose.yml` - Orchestration des services
- ✅ `README.md` - Documentation complète (ce fichier)
- ✅ Lien GitLab : https://gitlab.com/AymenMB1/iris-ai-service.git

---

## 👤 Auteur

- **Étudiant** - Aymen Mabrouk
Sous la direction de Dr. Salah Gontara

---

## 📝 Notes Importantes

1. **Ordre de construction:** L'API doit être saine (healthcheck) avant que le frontend ne démarre
2. **CORS:** L'API est configurée pour accepter les requêtes depuis `http://localhost:5174`
3. **Modèle ML:** Assurez-vous que `model.joblib` existe dans `api/app/model/`
4. **Build multi-stage:** Le frontend utilise une approche optimisée pour réduire la taille de l'image finale
5. **Monitoring:** Prometheus collecte automatiquement les métriques de l'API toutes les 5 secondes
6. **Grafana:** Le dashboard est automatiquement importé au démarrage avec la datasource Prometheus configurée
