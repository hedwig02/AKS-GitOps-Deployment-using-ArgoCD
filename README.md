# AKS GitOps Deployment using ArgoCD

## Project Overview

This project demonstrates a complete GitOps-based deployment workflow using Azure Kubernetes Service (AKS) and ArgoCD.

The application deployment is managed through GitHub repositories, where Git acts as the Single Source of Truth. ArgoCD continuously monitors the repository and automatically synchronizes changes to the AKS cluster.

This project follows modern DevOps and GitOps principles by eliminating manual Kubernetes deployments and ensuring the cluster state always matches the desired state stored in Git.

---

## Technologies Used

- Microsoft Azure
- Azure Kubernetes Service (AKS)
- Azure Container Registry (ACR)
- Kubernetes
- ArgoCD
- GitHub
- NGINX
- Azure Cloud Shell

---

## Architecture

Developer
↓
GitHub Repository
↓
ArgoCD
↓
AKS Cluster
↓
NGINX Deployment
↓
LoadBalancer Service
↓
Public IP

---

## Project Objectives

- Deploy an AKS Cluster
- Configure Azure Container Registry
- Install ArgoCD on AKS
- Create Kubernetes Deployment and Service Manifests
- Store Kubernetes manifests in GitHub
- Connect ArgoCD to GitHub repository
- Enable Continuous Synchronization
- Demonstrate GitOps deployment workflow
- Verify automated updates through Git commits

---

## Azure Resources Created

### Resource Group

```
rg-aks-gitops-sejal
```

### AKS Cluster

```
aks-gitops-sejal
```

### Azure Container Registry

```
sejalgitopsacr2026
```

---

## Repository Structure

```text
aks-manifests-sejal/
│
├── deployment.yaml
├── service.yaml
└── README.md
```

---

## Deployment Manifest

### deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment

spec:
  replicas: 3

  selector:
    matchLabels:
      app: nginx

  template:
    metadata:
      labels:
        app: nginx

    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
```

---

### service.yaml

```yaml
apiVersion: v1
kind: Service

metadata:
  name: nginx-service

spec:
  type: LoadBalancer

  selector:
    app: nginx

  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
```

---

## ArgoCD Installation

Install ArgoCD Namespace

```bash
kubectl create namespace argocd
```

Install ArgoCD

```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Verify Pods

```bash
kubectl get pods -n argocd
```

Expose ArgoCD Server

```bash
kubectl patch svc argocd-server -n argocd \
-p '{"spec":{"type":"LoadBalancer"}}'
```

Get External IP

```bash
kubectl get svc -n argocd
```

---

## AKS Connection

```bash
az aks get-credentials \
--resource-group rg-aks-gitops-sejal \
--name aks-gitops-sejal
```

Verify Nodes

```bash
kubectl get nodes
```

---

## GitOps Validation

Initial deployment used:

```yaml
replicas: 2
```

Modified deployment:

```yaml
replicas: 3
```

After committing changes to GitHub:

```bash
kubectl get deployments
```

Result:

```text
nginx-deployment   3/3
```

ArgoCD automatically synchronized the AKS cluster without any manual deployment commands.

---

## GitOps Benefits

- Single Source of Truth
- Automated Deployments
- Self-Healing Infrastructure
- Version Controlled Configuration
- Easy Rollback
- Continuous Synchronization
- Reduced Human Error

---

## Learning Outcomes

Through this project I gained hands-on experience with:

- Azure Kubernetes Service
- GitOps Architecture
- ArgoCD Continuous Delivery
- Kubernetes Deployments
- Kubernetes Services
- Azure Container Registry
- AKS Cluster Management
- GitHub Integration
- Infrastructure Automation

---

## Author

Sejal Sakhala

Azure Cloud & DevOps Engineer (Aspiring)

GitHub:
https://github.com/hedwig02
