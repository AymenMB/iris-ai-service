# 🚀 Guide de Démarrage Rapide - TP2 MLOps

## Vérification Préalable

✅ Docker Desktop est installé et démarré  
✅ Git est installé  
✅ Le dépôt est cloné dans: `d:\cycleing\5eme\MLOPS\TP2\iris-ai-service`

---

## Option 1 : Lancement Rapide avec Docker Compose (Recommandé)

```powershell
# Naviguer vers le projet
cd d:\cycleing\5eme\MLOPS\TP2\iris-ai-service

# Lancer l'application complète
docker compose up --build

# Une fois démarré, ouvrir dans le navigateur:
# - API Swagger: http://localhost:8000/docs
# - Frontend: http://localhost:5174
```

Pour arrêter (Ctrl+C puis):
```powershell
docker compose down
```

---

## Option 2 : Tests Individuels (Pour Validation)

### Test API Seule

```powershell
cd d:\cycleing\5eme\MLOPS\TP2\iris-ai-service\api

docker build -t iris-api:dev .
docker run -d -p 8000:8000 --name iris-api iris-api:dev

# Tester
curl http://localhost:8000/health

# Nettoyer
docker stop iris-api; docker rm iris-api
```

### Test Frontend Seul

```powershell
cd d:\cycleing\5eme\MLOPS\TP2\iris-ai-service\frontend

docker build -t iris-frontend:dev .
docker run -d -p 5174:80 --name iris-frontend iris-frontend:dev

# Ouvrir: http://localhost:5174

# Nettoyer
docker stop iris-frontend; docker rm iris-frontend
```

---

## 📊 Commandes Utiles

```powershell
# Voir les conteneurs actifs
docker ps

# Voir les logs en temps réel
docker compose logs -f

# Redémarrer les services
docker compose restart

# Tout nettoyer
docker compose down -v --rmi all
```

---

## ✅ Fichiers Créés pour le TP

1. ✅ `api/Dockerfile`
2. ✅ `frontend/Dockerfile`
3. ✅ `docker-compose.yml`
4. ✅ `README_TP2.md` (documentation complète)
5. ✅ `QUICKSTART.md` (ce fichier)

---

## 🎯 Points d'Accès

Une fois l'application lancée:

- **API Health:** http://localhost:8000/health
- **API Swagger:** http://localhost:8000/docs
- **Frontend:** http://localhost:5174

---

## 🐛 Problèmes Courants

### Port déjà utilisé
```powershell
# Trouver le processus sur le port 8000
netstat -ano | findstr :8000
# Tuer le processus (remplacer PID)
taskkill /PID <PID> /F
```

### Docker ne démarre pas
- Vérifier que Docker Desktop est lancé
- Redémarrer Docker Desktop

### Rebuild complet
```powershell
docker compose down -v
docker compose build --no-cache
docker compose up
```

---

## 📝 Pour Rendre le TP

1. Les 3 fichiers créés: `api/Dockerfile`, `frontend/Dockerfile`, `docker-compose.yml`
2. Le fichier `README_TP2.md` (documentation)
3. Captures d'écran de:
   - `docker ps` montrant les 2 conteneurs
   - Swagger UI (http://localhost:8000/docs)
   - Frontend (http://localhost:5174)
4. Votre fork GitLab personnel
