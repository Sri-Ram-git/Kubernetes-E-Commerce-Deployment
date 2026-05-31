<div align="center">

<img src="https://raw.githubusercontent.com/kubernetes/kubernetes/master/logo/logo.svg" width="100" alt="Kubernetes Logo"/>

# 🚀 Kubernetes E-Commerce Deployment

### Deploying & Managing a Production-Ready Static Website Using Docker · Kubernetes · Minikube · NGINX

[![Kubernetes](https://img.shields.io/badge/Kubernetes-1.29-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![Docker](https://img.shields.io/badge/Docker-24.0-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![NGINX](https://img.shields.io/badge/NGINX-1.25-009639?style=for-the-badge&logo=nginx&logoColor=white)](https://nginx.org/)
[![Minikube](https://img.shields.io/badge/Minikube-1.32-F6C915?style=for-the-badge&logo=kubernetes&logoColor=black)](https://minikube.sigs.k8s.io/)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)
[![Stars](https://img.shields.io/github/stars/yourusername/k8s-ecommerce?style=for-the-badge&color=FFD700)](https://github.com/yourusername/k8s-ecommerce/stargazers)

<br/>

> **A complete, hands-on DevOps project** demonstrating container orchestration, horizontal autoscaling, self-healing, and service networking — built for Cloud Engineering learners and recruiters evaluating real-world Kubernetes skills.

<br/>

[🔍 View Architecture](#-system-architecture) · [⚡ Quick Start](#-quick-start) · [📋 Implementation](#-step-by-step-implementation) · [🎯 Features](#-kubernetes-features-demonstrated) · [📊 Results](#-project-achievements)

</div>

---

## 📌 Table of Contents

- [Overview](#-overview)
- [Why Kubernetes?](#-why-kubernetes)
- [System Architecture](#-system-architecture)
- [Request Flow Diagram](#-request-flow)
- [Technologies Used](#-technologies-used)
- [Project Structure](#-project-structure)
- [Quick Start](#-quick-start)
- [Step-by-Step Implementation](#-step-by-step-implementation)
  - [Phase 1 — Docker Containerization](#phase-1--docker-containerization)
  - [Phase 2 — Kubernetes Deployment](#phase-2--kubernetes-deployment)
  - [Phase 3 — Scaling & Self-Healing](#phase-3--scaling--self-healing)
  - [Phase 4 — Horizontal Pod Autoscaler](#phase-4--horizontal-pod-autoscaler-hpa)
- [Kubernetes Features Demonstrated](#-kubernetes-features-demonstrated)
- [YAML Configuration Files](#-yaml-configuration-files)
- [Challenges & Solutions](#-challenges--solutions)
- [Project Achievements](#-project-achievements)
- [Learning Outcomes](#-learning-outcomes)
- [Future Improvements](#-future-improvements)
- [Author](#-author)

---

## 🌟 Overview

This project provides a **complete, production-inspired workflow** for deploying a containerized e-commerce website on Kubernetes. It covers every critical concept that modern cloud engineers use daily — from writing a Dockerfile to configuring an HPA that auto-scales your deployment based on CPU load.

```
✅ Docker Image Build      ✅ Kubernetes Deployment     ✅ NodePort Service
✅ Manual Scaling          ✅ Self-Healing Demo          ✅ HPA Autoscaling
✅ Minikube Local Cluster  ✅ K8s Dashboard              ✅ YAML-driven IaC
```

**Why this project stands out:**
- 🔄 **Real DevOps workflow** — not just theory; every command is production-patterned
- 📈 **Autoscaling** — HPA scales pods from 2 → 10 based on CPU utilization
- 🩺 **Self-Healing** — demonstrates Kubernetes restoring pod count automatically
- 🏗️ **Infrastructure as Code** — all resources defined in version-controlled YAML

---

## 🤔 Why Kubernetes?

| Without Kubernetes | With Kubernetes |
|---|---|
| Manual container restart on crash | ♻️ Automatic self-healing |
| SSH into servers to scale manually | 📈 One command or auto-scale |
| Complex load balancing setup | ⚖️ Built-in service load balancing |
| No rollback strategy | 🔄 Rolling updates + instant rollback |
| Config scattered across servers | 📋 Declarative YAML manifests |
| Downtime during deployments | 🟢 Zero-downtime deployments |

---

## 🏗️ System Architecture

```
                        ┌──────────────────────────────────────────┐
                        │           KUBERNETES CLUSTER              │
                        │                                          │
    ┌──────────┐        │   ┌───────────────────────────────────┐  │
    │   User   │──HTTP──┼──▶│     Kubernetes NodePort Service   │  │
    │ Browser  │        │   │        (Port 30007 → 80)          │  │
    └──────────┘        │   └──────────────┬────────────────────┘  │
                        │                  │ Load Balances          │
                        │        ┌─────────┼─────────┐             │
                        │        ▼         ▼         ▼             │
                        │   ┌─────────┐ ┌──────┐ ┌──────┐         │
                        │   │  Pod 1  │ │Pod 2 │ │Pod N │ ← HPA   │
                        │   │ NGINX   │ │NGINX │ │NGINX │  (2-10)  │
                        │   └────┬────┘ └──┬───┘ └──┬───┘         │
                        │        │          │         │             │
                        │   ┌────▼──────────▼─────────▼────────┐   │
                        │   │         Docker Containers         │   │
                        │   │    ecommerce-site:v1 (NGINX)      │   │
                        │   └───────────────────────────────────┘   │
                        │                                           │
                        │   ┌────────────────────────────────────┐  │
                        │   │     Static Website (HTML/CSS)       │  │
                        │   │  /usr/share/nginx/html/index.html   │  │
                        │   └────────────────────────────────────┘  │
                        └──────────────────────────────────────────┘
```

---

## 🔄 Request Flow

```
Browser Request
      │
      ▼
┌─────────────────────────────┐
│  minikube ip : 30007         │  ← NodePort opens external access
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│   ecommerce-service          │  ← Selects pods by label: app=ecommerce-site
│   (Kubernetes Service)       │
└──────┬──────────────┬────────┘
       │              │           ← Round-robin load balancing
       ▼              ▼
┌────────────┐  ┌────────────┐
│   Pod 1    │  │   Pod 2    │  ← Each pod runs one Docker container
│ (NGINX)    │  │ (NGINX)    │
└─────┬──────┘  └─────┬──────┘
      │                │
      ▼                ▼
┌─────────────────────────────┐
│   Static Website Files       │  ← index.html + style.css served
│   /usr/share/nginx/html/     │
└─────────────────────────────┘
```

---

## 🛠️ Technologies Used

| Technology | Version | Role | Why This Choice |
|---|---|---|---|
| 🐳 **Docker** | 24.0+ | Containerization | Industry standard; packages app + deps into portable images |
| ☸️ **Kubernetes** | 1.29+ | Orchestration | Auto-manages container lifecycle, scaling, networking |
| 🖥️ **Minikube** | 1.32+ | Local K8s cluster | Run full K8s locally; no cloud cost for learning |
| ⌨️ **kubectl** | Latest | K8s CLI | Apply YAML, inspect pods, scale deployments |
| 🌐 **NGINX** | Alpine | Web server | Lightweight, production-grade static file serving |
| 📄 **HTML/CSS** | — | Frontend | Static e-commerce UI to containerize |

---

## 📁 Project Structure

```
k8s-ecommerce/
│
├── 📄 Dockerfile                 # Container image definition
│
├── 🌐 website/
│   ├── index.html                # E-commerce homepage
│   ├── style.css                 # Responsive styles
│   └── images/                  # Product images & assets
│
├── ☸️  k8s/
│   ├── deployment.yaml           # Pod template, replicas, image config
│   ├── service.yaml              # NodePort service for external access
│   ├── ingress.yaml              # (Future) Ingress controller config
│   └── hpa.yaml                 # Horizontal Pod Autoscaler (CPU-based)
│
├── 📜 scripts/
│   └── deploy.sh                 # One-command deployment script
│
└── 📋 README.md                  # This file
```

---

## ⚡ Quick Start

> **Prerequisites:** [Docker](https://docs.docker.com/get-docker/), [Minikube](https://minikube.sigs.k8s.io/docs/start/), [kubectl](https://kubernetes.io/docs/tasks/tools/)

```bash
# 1. Clone the repository
git clone https://github.com/yourusername/k8s-ecommerce.git
cd k8s-ecommerce

# 2. Start Minikube
minikube start

# 3. Build and load the Docker image
docker build -t ecommerce-site:v1 .
minikube image load ecommerce-site:v1

# 4. Deploy everything
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml

# 5. Open in browser
minikube service ecommerce-service
```

**That's it.** Your e-commerce site is now running on Kubernetes. 🎉

---

## 📋 Step-by-Step Implementation

### Phase 1 — Docker Containerization

#### Step 1 · Write the Dockerfile

```dockerfile
# Use lightweight NGINX Alpine as base image
FROM nginx:alpine

# Copy website files into NGINX's default serve directory
COPY website/ /usr/share/nginx/html

# Open port 80 for HTTP traffic
EXPOSE 80
```

> **Why `nginx:alpine`?**  The Alpine variant is only ~23MB vs ~130MB for the full Debian image — smaller images pull faster in CI/CD and reduce attack surface.

#### Step 2 · Build the Image

```bash
docker build -t ecommerce-site:v1 .
```

| Flag | Meaning |
|---|---|
| `-t ecommerce-site:v1` | Tag the image with name and version |
| `.` | Use current directory as build context |

#### Step 3 · Test Locally

```bash
docker run -d -p 8080:80 ecommerce-site:v1
```

Open `http://localhost:8080` — if the website loads, Docker is working correctly ✅

---

### Phase 2 — Kubernetes Deployment

#### Step 4 · Start Minikube

```bash
minikube start
# Verify cluster is running
kubectl cluster-info
kubectl get nodes
```

#### Step 5 · Load Image into Minikube

```bash
# IMPORTANT: Without this, K8s tries to pull from Docker Hub and fails
minikube image load ecommerce-site:v1
```

#### Step 6 · Apply Deployment

```bash
kubectl apply -f k8s/deployment.yaml

# Watch pods come online
kubectl get pods --watch
```

**Expected output:**
```
NAME                                     READY   STATUS    RESTARTS   AGE
ecommerce-deployment-7d4f5b6c8-abc12    1/1     Running   0          30s
ecommerce-deployment-7d4f5b6c8-def34    1/1     Running   0          30s
```

#### Step 7 · Deploy the Service

```bash
kubectl apply -f k8s/service.yaml

# Get the URL to access the website
minikube service ecommerce-service --url
```

---

### Phase 3 — Scaling & Self-Healing

#### Manual Scaling

```bash
# Scale UP to handle traffic spike
kubectl scale deployment ecommerce-deployment --replicas=5

# Verify 5 pods are running
kubectl get pods
```

```
NAME                                     READY   STATUS    RESTARTS   AGE
ecommerce-deployment-7d4f5b6c8-abc12    1/1     Running   0          5m
ecommerce-deployment-7d4f5b6c8-def34    1/1     Running   0          5m
ecommerce-deployment-7d4f5b6c8-ghi56    1/1     Running   0          12s  ← new
ecommerce-deployment-7d4f5b6c8-jkl78    1/1     Running   0          12s  ← new
ecommerce-deployment-7d4f5b6c8-mno90    1/1     Running   0          12s  ← new
```

#### Self-Healing Demonstration

```bash
# Get pod name
kubectl get pods

# Delete a pod (simulates a crash)
kubectl delete pod ecommerce-deployment-7d4f5b6c8-abc12

# Watch Kubernetes immediately recreate it
kubectl get pods --watch
```

```
NAME                                     READY   STATUS        RESTARTS   AGE
ecommerce-deployment-7d4f5b6c8-abc12    1/1     Terminating   0          6m
ecommerce-deployment-7d4f5b6c8-pqr11    0/1     Pending       0          2s   ← auto-created!
ecommerce-deployment-7d4f5b6c8-pqr11    1/1     Running       0          5s   ← back up!
```

> **Key concept:** Kubernetes constantly reconciles **desired state** (5 replicas) with **actual state** (4 running). The moment it detects a discrepancy, it acts.

---

### Phase 4 — Horizontal Pod Autoscaler (HPA)

#### Step 8 · Enable Metrics Server

```bash
# Required for HPA to monitor CPU usage
minikube addons enable metrics-server

# Verify it's running
kubectl get pods -n kube-system | grep metrics
```

#### Step 9 · Apply the HPA

```bash
kubectl apply -f k8s/hpa.yaml

# Monitor autoscaling in real time
kubectl get hpa --watch
```

```
NAME             REFERENCE                          TARGETS   MINPODS   MAXPODS   REPLICAS
ecommerce-hpa    Deployment/ecommerce-deployment    12%/50%   2         10        2
```

When CPU > 50%, Kubernetes automatically scales pods up to a maximum of 10.

#### Step 10 · Open Dashboard

```bash
minikube dashboard
```

Visual real-time monitoring of pods, deployments, HPA events, and resource usage.

---

## 🎯 Kubernetes Features Demonstrated

### 📈 Scalability
Scale from 2 to 10 pods with a single command or automatically based on load. Kubernetes handles scheduling new pods on available node resources.

```bash
kubectl scale deployment ecommerce-deployment --replicas=10
```

### 🩺 Self-Healing
The `ReplicaSet` controller continuously monitors pod count. If any pod crashes, exits, or is deleted, Kubernetes recreates it automatically — typically within 5 seconds.

### ⚖️ Load Balancing
The `ecommerce-service` (type: NodePort) distributes incoming HTTP requests across all healthy pods using round-robin load balancing. No external load balancer required.

### 🤖 Horizontal Pod Autoscaling
The HPA watches CPU metrics via the Metrics Server. If average CPU across pods exceeds 50%, new pods are provisioned. When load drops, pods are terminated — you only use what you need.

```
CPU Usage 20% → 2 pods (min)
CPU Usage 55% → 4 pods  (scale up triggered)
CPU Usage 80% → 8 pods  (continues scaling)
CPU Usage 90% → 10 pods (max reached)
CPU Usage 15% → 2 pods  (scale back down)
```

### 🔄 Declarative Infrastructure
All cluster state is defined in YAML files committed to version control. Any team member can reproduce the exact environment with `kubectl apply -f k8s/`.

---

## 📄 YAML Configuration Files

### `k8s/deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: ecommerce-deployment

spec:
  replicas: 2                        # Start with 2 pods

  selector:
    matchLabels:
      app: ecommerce-site             # Must match template labels

  template:
    metadata:
      labels:
        app: ecommerce-site

    spec:
      containers:
      - name: ecommerce-container
        image: ecommerce-site:v1      # Our built Docker image
        imagePullPolicy: Never        # Use local image (not Docker Hub)
        ports:
        - containerPort: 80           # NGINX listens on port 80
```

### `k8s/service.yaml`

```yaml
apiVersion: v1
kind: Service

metadata:
  name: ecommerce-service

spec:
  selector:
    app: ecommerce-site              # Routes to pods with this label

  ports:
    - protocol: TCP
      port: 80                       # Service port (internal)
      targetPort: 80                 # Container port
      nodePort: 30007                # External access port

  type: NodePort                     # Exposes service on each Node's IP
```

### `k8s/hpa.yaml`

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler

metadata:
  name: ecommerce-hpa

spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: ecommerce-deployment

  minReplicas: 2                     # Always keep at least 2 pods
  maxReplicas: 10                    # Never exceed 10 pods

  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50       # Scale when avg CPU > 50%
```

---

## 🧩 Challenges & Solutions

| # | Challenge | Root Cause | Solution Applied |
|---|---|---|---|
| 1 | `COPY website/` failed in Dockerfile | Dockerfile was placed inside a subdirectory, causing build context mismatch | Moved Dockerfile to project root so all paths resolve from root |
| 2 | `ImagePullBackOff` error on pods | Kubernetes tried to pull image from Docker Hub since it wasn't in Minikube's registry | Added `imagePullPolicy: Never` + ran `minikube image load ecommerce-site:v1` |
| 3 | HPA showed `<unknown>/50%` for CPU target | Metrics Server addon was not enabled in Minikube | Ran `minikube addons enable metrics-server` and waited for it to come online |

---

## 📊 Project Achievements

| Milestone | Status | Demonstrated Via |
|---|---|---|
| Docker image build | ✅ Done | `docker build` + `docker run` |
| Kubernetes deployment | ✅ Done | `kubectl apply -f deployment.yaml` |
| Multi-pod management | ✅ Done | `kubectl get pods` (2 replicas) |
| Service networking | ✅ Done | `minikube service ecommerce-service` |
| Manual scaling | ✅ Done | `kubectl scale --replicas=5` |
| Self-healing | ✅ Done | `kubectl delete pod` → auto-recreated |
| CPU-based autoscaling | ✅ Done | HPA config + metrics-server |
| Kubernetes dashboard | ✅ Done | `minikube dashboard` |

---

## 🎓 Learning Outcomes

After building this project, you will have hands-on experience with:

```
🐳 Docker          → Build images, run containers, understand layers
☸️  Kubernetes     → Deployments, ReplicaSets, Services, HPA
📄 YAML            → Declarative infrastructure configuration
🌐 Networking      → NodePort, pod-to-pod communication, label selectors
📈 Scaling         → Manual scaling + CPU-triggered autoscaling
🩺 Reliability     → Self-healing, desired-state reconciliation
🔍 Observability   → kubectl logs, describe, dashboard monitoring
⚙️  Minikube       → Local cluster management and addons
```

These are **core skills tested in DevOps, Cloud Engineer, and SRE interviews.**

---

## 🚀 Future Improvements

This project is designed as a foundation. The next steps to make it production-ready:

- [ ] **Node.js Backend** — Add REST APIs for product catalog and cart management
- [ ] **Database Layer** — PostgreSQL with Persistent Volume Claims (PVC) for data durability
- [ ] **Ingress Controller** — NGINX Ingress with domain routing + TLS/SSL via cert-manager
- [ ] **CI/CD Pipeline** — GitHub Actions workflow to build, push to Docker Hub, and auto-deploy
- [ ] **Helm Charts** — Package all K8s manifests into a reusable, parameterized Helm chart
- [ ] **Monitoring Stack** — Prometheus for metrics collection + Grafana dashboards
- [ ] **Resource Limits** — Add CPU/memory requests and limits to all containers
- [ ] **Health Checks** — Liveness and readiness probes for zero-downtime deployments

---

## 📚 References & Resources

- [Kubernetes Official Docs](https://kubernetes.io/docs/)
- [Docker Documentation](https://docs.docker.com/)
- [Minikube Getting Started](https://minikube.sigs.k8s.io/docs/start/)
- [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
- [NGINX Docker Hub](https://hub.docker.com/_/nginx)

---

## 👤 Author

**Your Name**
- 🔗 LinkedIn: [linkedin.com/in/es-sriram](https://linkedin.com/in/es-sriram)
- 🐙 GitHub: [github.com/Sri-Ram-git](https://github.com/Sri-Ram-git)
- 📧 Email: sriram.es.contact@gmail.com

---

<div align="center">

### ⭐ If this project helped you learn Kubernetes, please give it a star!

**Stars help other learners find this project and motivate continued improvements.**

[![Star this repo](https://img.shields.io/github/stars/Sri-Ram-git/Kubernetes-E-Commerce-Deployment?style=social)](https://github.com/Sri-Ram-git/Kubernetes-E-Commerce-Deployment)
[![Fork this repo](https://img.shields.io/github/forks/Sri-Ram-git/Kubernetes-E-Commerce-Deployment?style=social)](https://github.com/Sri-Ram-git/Kubernetes-E-Commerce-Deployment/fork)
[![Watch this repo](https://img.shields.io/github/watchers/Sri-Ram-git/Kubernetes-E-Commerce-Deployment?style=social)](https://github.com/Sri-Ram-git/Kubernetes-E-Commerce-Deployment/watchers)

<br/>

*Built with ❤️ to help developers learn production DevOps workflows*

</div>
