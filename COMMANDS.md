# 📝 Commandes utiles

## k3s

```bash
# Status
sudo systemctl status k3s

# Restart
sudo systemctl restart k3s

# Logs
sudo journalctl -u k3s -f
```

## kubectl

```bash
# Tous les pods
kubectl get pods -A

# Pods par namespace
kubectl get pods -n gitlab
kubectl get pods -n vault
kubectl get pods -n argocd

# Logs d'un pod
kubectl logs -f <pod-name> -n <namespace>

# Describe un pod
kubectl describe pod <pod-name> -n <namespace>
```

## GitLab

```bash
# Port-forward GitLab UI
kubectl port-forward -n gitlab svc/gitlab-webservice-default 8080:8181
```

## ArgoCD

```bash
# Récupérer le mot de passe
kubectl -n argocd get secret argocd-initial-admin-secret \
  -o jsonpath="{.data.password}" | base64 -d
```

## Vault

```bash
# Exec dans le pod Vault
kubectl exec -n vault -it vault-0 -- /bin/sh

# List secrets
vault kv list secret/ytech/

# Get secret
vault kv get secret/ytech/database
```
