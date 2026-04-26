# 🚀 Guide de démarrage rapide

## Installation en 5 minutes

### 1. Installer k3s

```bash
curl -sfL https://get.k3s.io | sh -
mkdir -p ~/.kube
sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config
sudo chown $USER:$USER ~/.kube/config
```

### 2. Déployer l'application

```bash
kubectl apply -f k8s-manifests/
```

### 3. Vérifier le déploiement

```bash
kubectl get pods -l app=ytech-app
kubectl get svc ytech-app-service
```

### 4. Accéder à l'application

```bash
curl http://localhost:30808
```

## Prochaines étapes

- 📚 Lire le [README complet](README.md)
- 🎬 Lancer le [script de démo](scripts/demo-ytech-complete.sh)
- 📖 Consulter la [documentation](docs/)
