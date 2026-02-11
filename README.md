# Kubernetes GitOps Homelab

A production-inspired Kubernetes homelab project designed to demonstrate GitOps practices using ArgoCD, Jenkins, and Kustomize.

This repository simulates a real-world DevOps/SRE environment, focusing on infrastructure-as-code, automation, and scalable platform design.

⚠️ Note: This project is designed for local environments and learning purposes. It intentionally simulates production-like workflows without requiring cloud resources.

🌐 ngrok is used to expose ArgoCD externally for webhook integration.

---

## 🚀 Overview

This project implements a GitOps-based Kubernetes platform architecture running locally (Docker Desktop Kubernetes), where all infrastructure and applications are managed declaratively via Git and synchronized by ArgoCD.

### Key Concepts

- GitOps with ArgoCD
- Kubernetes manifests and Kustomize
- Infrastructure as Code (IaC)
- Local homelab environment
- DevOps tooling integration (Jenkins)
- Extensible platform architecture

---

## 🏗️ Architecture

```

Developer
   ↓
GitHub Repository (GitOps)
   ↓
ArgoCD
   ↓
Kubernetes Cluster (Local Homelab)
   ├── Platform API (Go)
   ├── Ephemeral Environment Operator (Helm + CRDs)
   ├── Frontend Dashboard (React)
   ├── Jenkins
   ├── Prometheus / Grafana
   └── Ingress / Exposure (ngrok / Traefik – optional)

```

---

## 📂 Repository Structure

```

.
├── argocd/
│   ├── argocd-app.yaml
│   └── apps/
│       ├── infra/
│       └── tools/
├── tools/
│   └── jenkins/
├── secrets/   # ignored by git (.gitignore)
├── .gitignore
└── README.md

````

---

## 🛠️ Prerequisites

- Docker Desktop with Kubernetes enabled
- kubectl
- Git

---

## ⚙️ Local Cluster Setup

### 1) Enable Kubernetes in Docker Desktop

Make sure Kubernetes is enabled in Docker Desktop settings.

---

### 2) Create namespaces

```bash
kubectl create namespace argocd
kubectl create namespace ngrok
````

---

### 3) Install ArgoCD (server-side apply)

```bash
kubectl apply --server-side -n argocd \
  -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

---

### 4) Update the secrets and config-maps

```bash
kubectl apply -n argocd -f secrets/argocd-secret.yaml
kubectl apply -n argocd -f argocd/argocd-cm.yaml
kubectl apply -n ngrok -f secrets/ngrok-secret.yaml
```

---

### 5) Restart argocd deployments

```bash
kubectl -n argocd rollout restart statefulset argocd-application-controller
kubectl -n argocd rollout restart deployment argocd-repo-server
kubectl -n argocd rollout restart deployment argocd-server
```

---

### 6) Deploy the root ArgoCD application (GitOps bootstrap)

```bash
kubectl apply -n argocd -f argocd/argocd-app.yaml
```

---

### 7) Access ArgoCD UI

Get the admin password:

```bash
kubectl -n argocd get secret argocd-initial-admin-secret \
  -o jsonpath="{.data.password}" | base64 -d
```

Port-forward ArgoCD:

```bash
kubectl -n argocd port-forward svc/argocd-server 8080:443
```

Access:

```
https://localhost:8080
```

---

## 🔧 Jenkins Deployment

Jenkins is deployed automatically by ArgoCD using Kubernetes manifests and Kustomize.

Location:

```
tools/jenkins/
```

---

## 🧩 Roadmap

Planned improvements:

* [ ] Secrets management (Sealed Secrets / External Secrets)
* [ ] Helm-based environment provisioning
* [ ] Kubernetes Operator (CRDs + controller)
* [ ] Platform API in Go for ephemeral environments
* [ ] React frontend for platform management
* [ ] GitHub Actions integration with ArgoCD
* [ ] Observability stack (Prometheus / Grafana)
* [ ] Local ingress strategies (ngrok / Traefik – optional)

---

## 🎯 Purpose

This project was created to:

* Demonstrate practical DevOps and SRE skills
* Showcase GitOps workflows in Kubernetes
* Serve as a personal platform engineering lab
* Provide a portfolio-ready reference architecture

---

## 👤 Author

Kelvin Alef
Site Reliability Engineer | DevOps & Cloud Specialist | Senior Java Developer

* LinkedIn: [https://linkedin.com/in/kelvin-alef](https://linkedin.com/in/kelvin-alef)
* GitHub: [https://github.com/kelvin-alef](https://github.com/kelvin-alef)

---

## ⭐ If you find this project useful, feel free to star the repository!

```
