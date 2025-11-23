# 🎮 Commandes Docker - Aide-Mémoire TP2

## 📁 Emplacement du Projet
```powershell
cd d:\cycleing\5eme\MLOPS\TP2\iris-ai-service
```

---

## 🚀 Commandes Principales

### Lancer l'Application Complète
```powershell
docker compose up --build
```

### Lancer en Arrière-Plan (Detached)
```powershell
docker compose up --build -d
```

### Arrêter l'Application
```powershell
# Avec Ctrl+C si en mode interactif, puis:
docker compose down
```

### Arrêter et Nettoyer Complètement
```powershell
docker compose down -v --rmi all
```

---

## 🔍 Surveillance et Logs

### Voir les Conteneurs Actifs
```powershell
docker ps
```

### Voir Tous les Conteneurs (y compris arrêtés)
```powershell
docker ps -a
```

### Logs en Temps Réel
```powershell
# Tous les services
docker compose logs -f

# API uniquement
docker compose logs -f api

# Frontend uniquement
docker compose logs -f frontend
```

### Logs des Dernières Lignes
```powershell
docker compose logs --tail=50
```

---

## 🔄 Gestion des Services

### Redémarrer un Service
```powershell
docker compose restart api
docker compose restart frontend
```

### Arrêter un Service Spécifique
```powershell
docker compose stop api
docker compose stop frontend
```

### Démarrer un Service Spécifique
```powershell
docker compose start api
docker compose start frontend
```

---

## 🏗️ Build et Images

### Reconstruire Sans Cache
```powershell
docker compose build --no-cache
```

### Reconstruire un Service Spécifique
```powershell
docker compose build api
docker compose build frontend
```

### Voir les Images Docker
```powershell
docker images
```

### Supprimer une Image
```powershell
docker rmi iris-api:dev
docker rmi iris-frontend:dev
```

---

## 🧹 Nettoyage

### Supprimer les Conteneurs Arrêtés
```powershell
docker container prune
```

### Supprimer les Images Inutilisées
```powershell
docker image prune
```

### Supprimer les Volumes Inutilisés
```powershell
docker volume prune
```

### Nettoyage Complet du Système
```powershell
docker system prune -a --volumes
```

---

## 🔧 Debug et Inspection

### Entrer dans un Conteneur
```powershell
# API
docker exec -it iris-api /bin/bash

# Frontend
docker exec -it iris-frontend /bin/sh
```

### Inspecter un Conteneur
```powershell
docker inspect iris-api
docker inspect iris-frontend
```

### Voir l'Utilisation des Ressources
```powershell
docker stats
```

### Voir les Processus dans un Conteneur
```powershell
docker top iris-api
docker top iris-frontend
```

---

## 🌐 Tests des Services

### Test API Health
```powershell
curl http://localhost:8000/health
```

### Test API Prédiction
```powershell
curl -X POST http://localhost:8000/predict `
  -H "Content-Type: application/json" `
  -d '{
    "sepal_length": 5.1,
    "sepal_width": 3.5,
    "petal_length": 1.4,
    "petal_width": 0.2
  }'
```

### Test Frontend
```powershell
# Ouvrir dans le navigateur
start http://localhost:5174
```

### Test API Swagger
```powershell
# Ouvrir dans le navigateur
start http://localhost:8000/docs
```

---

## 🔌 Gestion des Ports

### Vérifier un Port Utilisé
```powershell
netstat -ano | findstr :8000
netstat -ano | findstr :5174
```

### Tuer un Processus sur un Port
```powershell
# Remplacer <PID> par l'ID du processus
taskkill /PID <PID> /F
```

---

## 📦 Tests Individuels (Sans Docker Compose)

### Test API Seule
```powershell
cd api
docker build -t iris-api:dev .
docker run -d -p 8000:8000 --name iris-api iris-api:dev

# Tester
curl http://localhost:8000/health

# Nettoyer
docker stop iris-api
docker rm iris-api
```

### Test Frontend Seul
```powershell
cd frontend
docker build -t iris-frontend:dev .
docker run -d -p 5174:80 --name iris-frontend iris-frontend:dev

# Tester
start http://localhost:5174

# Nettoyer
docker stop iris-frontend
docker rm iris-frontend
```

---

## 🔍 Vérification du Setup

### Vérifier Docker
```powershell
docker --version
docker compose version
```

### Vérifier les Réseaux
```powershell
docker network ls
docker network inspect iris-ai-service_iris-network
```

### Vérifier les Volumes
```powershell
docker volume ls
```

---

## 📊 Points d'Accès

| Service | URL | Description |
|---------|-----|-------------|
| API Health | http://localhost:8000/health | Statut de l'API |
| API Swagger | http://localhost:8000/docs | Documentation interactive |
| API Redoc | http://localhost:8000/redoc | Documentation alternative |
| API Predict | http://localhost:8000/predict | Endpoint de prédiction |
| API Metrics | http://localhost:8000/metrics | Métriques Prometheus |
| Frontend | http://localhost:5174 | Interface utilisateur |

---

## 🎯 Workflow Recommandé

1. **Premier Lancement**
   ```powershell
   docker compose up --build
   ```

2. **Vérifier les Services**
   ```powershell
   docker ps
   ```

3. **Tester l'API**
   ```powershell
   curl http://localhost:8000/health
   start http://localhost:8000/docs
   ```

4. **Tester le Frontend**
   ```powershell
   start http://localhost:5174
   ```

5. **Voir les Logs**
   ```powershell
   docker compose logs -f
   ```

6. **Arrêter Proprement**
   ```powershell
   docker compose down
   ```

---

## ⚠️ En Cas de Problème

### Rebuild Complet
```powershell
docker compose down -v
docker compose build --no-cache
docker compose up
```

### Vérifier Docker Desktop
```powershell
# Ouvrir Docker Desktop et vérifier qu'il est bien démarré
```

### Vérifier les Logs d'Erreur
```powershell
docker compose logs api
docker compose logs frontend
```

---

## 📝 Notes Importantes

- **Ports requis:** 8000 (API) et 5174 (Frontend)
- **Réseau:** Les conteneurs communiquent via `iris-network`
- **Healthcheck:** L'API doit être "healthy" avant que le frontend ne démarre
- **CORS:** Configuré pour accepter les requêtes depuis http://localhost:5174
- **Rebuild:** Utilisez `--build` pour reconstruire après des modifications du code

---

**Bon courage pour votre TP! 🚀**
