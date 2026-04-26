# 🔐 Ytech Solutions - Stack DevSecOps Complète

[![ISO 27001](https://img.shields.io/badge/ISO_27001-2022-green.svg)](https://www.iso.org/standard/27001)
[![k3s](https://img.shields.io/badge/k3s-v1.34.6-blue.svg)](https://k3s.io/)
[![GitLab](https://img.shields.io/badge/GitLab-18.11.1-orange.svg)](https://gitlab.com/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

> **Projet de fin de formation - Formation Cybersécurité JobInTech**  
> Infrastructure DevSecOps complète avec CI/CD, GitOps, Supply Chain Security et conformité ISO 27001

---

## 📋 Table des matières

- [Vue d'ensemble](#-vue-densemble)
- [Architecture](#-architecture)
- [Stack technique](#-stack-technique)
- [Conformité ISO 27001](#-conformité-iso-27001)
- [Installation](#-installation)
- [Documentation](#-documentation)
- [Démo](#-démo)
- [Captures d'écran](#-captures-décran)
- [Auteur](#-auteur)

---

## 🎯 Vue d'ensemble

**Ytech Solutions** est une infrastructure DevSecOps complète construite pour une entreprise fictive de 24 employés répartis sur 6 départements. Le projet démontre l'implémentation d'une stack moderne de sécurité, d'automatisation et de conformité.

### Objectifs du projet

- ✅ Déployer une infrastructure Zero Trust complète
- ✅ Automatiser le pipeline CI/CD avec 10 jobs de sécurité
- ✅ Implémenter GitOps avec ArgoCD
- ✅ Sécuriser la supply chain avec Cosign
- ✅ Gérer les secrets avec HashiCorp Vault
- ✅ Atteindre la conformité ISO 27001:2022

### Résultats clés

- **93% de pods opérationnels** (25/27)
- **10/10 jobs CI/CD PASSED**
- **8/8 contrôles ISO 27001** implémentés
- **100% des composants critiques fonctionnels**

---

## 🏗️ Architecture

### Schéma global

![Architecture globale](diagrams/ytech_architecture_globale.svg)

### Composants principaux

| Composant | Version | Rôle | Status |
|-----------|---------|------|--------|
| **k3s** | v1.34.6 | Orchestration Kubernetes | ✅ Running |
| **GitLab CE** | 18.11.1 | CI/CD Platform | ✅ Running |
| **HashiCorp Vault** | 1.21.2 | Secrets Management | ✅ Running |
| **ArgoCD** | 3.3.8 | GitOps | ✅ Running |
| **Cosign** | 2.4.1 | Supply Chain Security | ✅ Active |

---

## 🔧 Stack technique

### Infrastructure

- **OS** : Ubuntu Server 24.04 LTS
- **Virtualisation** : VMware Workstation
- **Container Runtime** : containerd
- **Orchestration** : k3s (Lightweight Kubernetes)

### CI/CD Pipeline (10 jobs)

#### Stage 1️⃣ : BUILD
- `build-image` - Construction image Docker

#### Stage 2️⃣ : TEST  
- `test-unit` - Tests unitaires

#### Stage 3️⃣ : SECURITY (7 jobs)
- `security-dockerfile` - Hadolint (Dockerfile linting)
- `security-secrets` - GitLeaks (secrets scanning)
- `security-sast` - Bandit (SAST Python)
- `security-trivy-fs` - Trivy (filesystem scan)
- `security-deps` - Trivy (dependencies scan)
- `sign-artifact` - Cosign (signature cryptographique)
- `vault-secrets` - Vault (récupération secrets)

#### Stage 4️⃣ : DEPLOY
- `deploy-production` - Déploiement k8s

### Outils de sécurité

```
Hadolint    → Analyse Dockerfile
GitLeaks    → Détection secrets
Bandit      → SAST Python
Trivy       → Scan vulnérabilités (FS + deps)
Cosign      → Signature artifacts (ECDSA P-256)
Vault       → Gestion secrets (AES-256-GCM)
```

### GitOps

- **ArgoCD** : Déploiement continu depuis Git
- **Sync Status** : SYNCED + HEALTHY
- **Application** : ytech-app (2 replicas)

---

## 🛡️ Conformité ISO 27001

### Contrôles Annex A implémentés (8/8)

| ID | Contrôle | Outil | Implémentation |
|----|----------|-------|----------------|
| **A.5.15** | Access control | Vault | Policies AppRole |
| **A.5.17** | Authentication information | Vault | Secrets centralisés |
| **A.5.37** | Documented operating procedures | ArgoCD | Infrastructure as Code |
| **A.8.8** | Management of technical vulnerabilities | Trivy | Scans automatisés |
| **A.8.24** | Use of cryptography | Vault + Cosign | AES-256-GCM + ECDSA P-256 |
| **A.8.28** | Secure coding | Hadolint + Bandit | Linting + SAST |
| **A.8.30** | Outsourced development | Cosign | Signature artifacts |
| **A.8.32** | Change management | ArgoCD | GitOps workflow |

---

## 📦 Installation

### Prérequis

```bash
# OS
Ubuntu Server 24.04 LTS (4 CPU, 8GB RAM, 100GB disk)

# Réseau
Accès Internet pour téléchargements
IP statique : 192.168.1.128
```

### Installation k3s

```bash
# Installation k3s
curl -sfL https://get.k3s.io | sh -

# Configuration kubectl
mkdir -p ~/.kube
sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config
sudo chown $USER:$USER ~/.kube/config
chmod 600 ~/.kube/config

# Vérification
kubectl get nodes
```

### Déploiement GitLab (Helm)

```bash
# Ajouter le repo Helm GitLab
helm repo add gitlab https://charts.gitlab.io/
helm repo update

# Installer GitLab
helm install gitlab gitlab/gitlab \
  --namespace gitlab \
  --create-namespace \
  --set global.hosts.domain=ytech.local \
  --set global.edition=ce \
  --timeout 10m

# Attendre le déploiement (5-10 min)
kubectl get pods -n gitlab -w
```

### Déploiement Vault (Helm)

```bash
# Ajouter le repo Helm HashiCorp
helm repo add hashicorp https://helm.releases.hashicorp.com
helm repo update

# Installer Vault (mode dev)
helm install vault hashicorp/vault \
  --namespace vault \
  --create-namespace \
  --set server.dev.enabled=true

# Configurer AppRole
kubectl exec -n vault vault-0 -- vault auth enable approle
kubectl exec -n vault vault-0 -- vault write auth/approle/role/gitlab-ci \
  token_policies="gitlab-ci" \
  token_ttl=1h
```

### Déploiement ArgoCD

```bash
# Installer ArgoCD
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# Exposer l'UI (NodePort)
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'

# Récupérer le mot de passe admin
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

### Installation Cosign

```bash
# Télécharger Cosign
COSIGN_VERSION=v2.4.1
wget "https://github.com/sigstore/cosign/releases/download/${COSIGN_VERSION}/cosign-linux-amd64"
sudo mv cosign-linux-amd64 /usr/local/bin/cosign
sudo chmod +x /usr/local/bin/cosign

# Générer une paire de clés
cosign generate-key-pair

# Clés stockées : cosign.key (privée) + cosign.pub (publique)
```

---

## 📚 Documentation

### Documents disponibles

| Document | Description | Pages |
|----------|-------------|-------|
| [Documentation complète](docs/Ytech-DevSecOps-Documentation.docx) | Architecture, pipeline, conformité | 14 |
| [Guide captures](docs/Ytech-DevSecOps-Documentation-Screenshots.docx) | Instructions screenshots | 14 |
| [Présentation PowerPoint](presentations/Ytech-DevSecOps-Presentation.pptx) | Slides soutenance | 10 |
| [README Demo](scripts/README-DEMO.md) | Guide script démo | - |

### Diagrammes architecturaux

Tous les diagrammes sont disponibles dans le dossier [`diagrams/`](diagrams/) :

- 🏗️ `ytech_architecture_globale.svg` - Vue d'ensemble stack
- 🔄 `ytech_pipeline_cicd.svg` - Pipeline 10 jobs
- 🚀 `ytech_gitops_argocd.svg` - Flow GitOps
- 🔐 `ytech_vault_secrets_flow.svg` - Authentification Vault
- 🌐 `ytech_network_topology.svg` - Réseau complet (dual ISP, DMZ, VLANs)
- 🛡️ `ytech_siem_architecture.svg` - SIEM Wazuh + TheHive
- 💾 `ytech_backup_dr_strategy.svg` - Backup & DR Oracle Cloud

---

## 🎬 Démo

### Script automatisé

Un script de démo complet est disponible pour démontrer le fonctionnement de la stack :

```bash
# Lancer la démo complète (4-6 minutes)
./scripts/demo-ytech-complete.sh
```

**Contenu de la démo** :
1. Infrastructure k3s
2. Pods déployés (par namespace)
3. Pipeline GitLab CI/CD
4. Cosign - Supply Chain Security
5. HashiCorp Vault - Secrets Management
6. ArgoCD - GitOps
7. Test HTTP application
8. Métriques globales
9. URLs de référence

### URLs des interfaces

| Service | URL | Credentials |
|---------|-----|-------------|
| **GitLab** | http://192.168.1.128:30123 | - |
| **ArgoCD** | http://192.168.1.128:30880 | admin / g6nwXFdfKDvt40y5 |
| **Vault** | http://192.168.1.128:30820/ui | root token: ytech-root-token |
| **Application** | http://192.168.1.128:30808 | - |

---

## 📸 Captures d'écran

### Pipeline GitLab CI/CD

![Pipeline GitLab](docs/screenshots/pipeline-gitlab.png)

**Pipeline #8 - Status: PASSED (10/10 jobs)**

### ArgoCD - Application Status

![ArgoCD](docs/screenshots/argocd-app.png)

**Application ytech-app : SYNCED + HEALTHY**

### HashiCorp Vault - Secrets

![Vault](docs/screenshots/vault-secrets.png)

**3 secrets stockés et chiffrés (AES-256-GCM)**

---

## 📊 Métriques

### Infrastructure

```
Node ubuntuserv     : Ready
k3s version         : v1.34.6+k3s1
Namespaces          : 4 (gitlab, vault, argocd, default)
Pods total          : 27
Pods Running        : 25 (93%)
```

### Pipeline CI/CD

```
Total jobs          : 10
Jobs PASSED         : 10 (100%)
Durée moyenne       : ~45 secondes
Artifacts signés    : Oui (Cosign ECDSA P-256)
```

### Sécurité

```
Outils actifs       : 7
Secrets management  : Vault (3 secrets chiffrés)
Supply chain        : Cosign (signature cryptographique)
Scans automatisés   : Hadolint, GitLeaks, Bandit, Trivy×2
```

### Conformité

```
ISO 27001 Annex A   : 8/8 contrôles (100%)
Documentation       : Complète
Preuves             : Captures + logs
```

---

## 🗂️ Structure du projet

```
ytech-devsecops-project/
├── README.md                          # Ce fichier
├── docs/                              # Documentation
│   ├── Ytech-DevSecOps-Documentation.docx
│   ├── Ytech-DevSecOps-Documentation-Screenshots.docx
│   └── screenshots/                   # Captures d'écran
├── diagrams/                          # Diagrammes architecturaux SVG
│   ├── ytech_architecture_globale.svg
│   ├── ytech_pipeline_cicd.svg
│   ├── ytech_gitops_argocd.svg
│   ├── ytech_vault_secrets_flow.svg
│   ├── ytech_network_topology.svg
│   ├── ytech_siem_architecture.svg
│   └── ytech_backup_dr_strategy.svg
├── presentations/                     # Présentations
│   └── Ytech-DevSecOps-Presentation.pptx
├── scripts/                           # Scripts démo
│   ├── demo-ytech.sh                 # Version simple
│   ├── demo-ytech-complete.sh        # Version complète
│   └── README-DEMO.md                # Guide utilisation
├── k8s-manifests/                     # Manifestes Kubernetes
│   ├── deployment.yaml
│   └── service.yaml
├── configs/                           # Configurations
│   ├── .gitlab-ci.yml                # Pipeline GitLab
│   ├── vault-policies/               # Policies Vault
│   └── argocd-apps/                  # Applications ArgoCD
└── LICENSE
```

---

## 🚀 Utilisation

### Cloner le repository

```bash
git clone https://github.com/yourusername/ytech-devsecops-project.git
cd ytech-devsecops-project
```

### Déployer la stack

```bash
# 1. Installer k3s
curl -sfL https://get.k3s.io | sh -

# 2. Déployer les services
kubectl apply -f k8s-manifests/

# 3. Vérifier les déploiements
kubectl get pods -A
```

### Lancer la démo

```bash
# Copier le script sur le serveur k3s
scp scripts/demo-ytech-complete.sh user@192.168.1.128:~/

# SSH sur le serveur
ssh user@192.168.1.128

# Lancer la démo
~/demo-ytech-complete.sh
```

---

## 🤝 Contribution

Les contributions sont les bienvenues ! Veuillez :

1. Fork le projet
2. Créer une branche (`git checkout -b feature/AmazingFeature`)
3. Commit vos changements (`git commit -m 'Add AmazingFeature'`)
4. Push vers la branche (`git push origin feature/AmazingFeature`)
5. Ouvrir une Pull Request

---

## 📄 License

Ce projet est sous licence MIT. Voir le fichier [LICENSE](LICENSE) pour plus de détails.

---

## 👨‍💻 Auteur

**Abdelilah**  
Formation Cybersécurité - JobInTech  
Projet de fin de formation - 2026

### Contact

- 📧 Email : [votre-email@example.com]
- 💼 LinkedIn : [Votre profil LinkedIn]
- 🌐 Portfolio : [votre-portfolio.com]

---

## 🙏 Remerciements

- **JobInTech** - Formation Cybersécurité
- **Communauté k3s** - Documentation excellente
- **GitLab** - Plateforme CI/CD complète
- **HashiCorp** - Vault secrets management
- **Argoproj** - ArgoCD GitOps
- **Sigstore** - Cosign supply chain security

---

## 📈 Roadmap

- [ ] Ajouter monitoring avec Prometheus + Grafana
- [ ] Implémenter SIEM complet (Wazuh + TheHive)
- [ ] Déployer backup/DR sur Oracle Cloud
- [ ] Intégrer scanning images avec Trivy Operator
- [ ] Ajouter policy enforcement avec Kyverno
- [ ] Documentation API avec Swagger

---

## ⭐ Star History

Si ce projet vous a été utile, n'hésitez pas à lui donner une étoile ! ⭐

---

<div align="center">

**Fait avec ❤️ pour la communauté DevSecOps**

[![ISO 27001](https://img.shields.io/badge/ISO_27001-Compliant-green.svg)](https://www.iso.org/standard/27001)
[![DevSecOps](https://img.shields.io/badge/DevSecOps-Ready-blue.svg)](https://www.devsecops.org/)
[![Security](https://img.shields.io/badge/Security-First-red.svg)](https://owasp.org/)

</div>
