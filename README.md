# 🚀 GitOps DevOps Project with ArgoCD & Monitoring Stack

## 📌 Project Overview
- This project demonstrates a GitOps-based DevOps workflow using ArgoCD to manage Kubernetes deployments, along with a full observability and monitoring stack.
- The goal is to practice real-world DevOps practices such as CI/CD with GitHub Actions, GitOps deployments, and cloud-native monitoring/logging.

## ⚙️ Tools & Technologies
- **GitHub Actions** → CI/CD pipelines to build, test, scan, and push Docker images.
- **ArgoCD** → GitOps tool that continuously syncs Kubernetes cluster state with the Git repository.
- **kube-prometheus-stack** → A collection of monitoring tools (Prometheus + Grafana + Alertmanager) for metrics and alerts.
- **Prometheus** → Collects and stores time-series metrics from applications and Kubernetes components.
- **Grafana** → Visualizes metrics and logs in dashboards.
- **Loki** → Log aggregation system (like Prometheus but for logs).
- **Promtail** → Collects logs from pods/nodes and ships them to Loki.
- **NGINX Ingress Controller** → Manages external access to services inside the Kubernetes cluster.
- **Reloader** → Automatically restarts pods when ConfigMaps or Secrets change.
- **kube-prometheus CRDs** → Custom Resource Definitions that extend Kubernetes to define monitoring resources.

## 📂Project Structure 
```
├── infrastructure/           # Kubernetes YAML manifests
│   ├── ingress-nginx/
│   ├── kube-prometheus-stack/
│   ├── loki/
│   └── reloader/
|   └── promtail/             
└── app-1/
|   └── deployment/
└── apps/
|   └── infra/
|   └── app/
└── README.md
└── argocd/
```
## 🚀 Getting Started
**Create the first app:**
```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: argocd
  namespace: argocd
spec:
  project: default
  source:
    repoURL: 'git@github.com:AhmedAmrgg/simple-gitops.git'
    path: apps
    targetRevision: main
  destination:
    server: 'https://kubernetes.default.svc'
    namespace: argocd
  syncPolicy:
    automated:
      selfHeal: true
      prune: true
```
```
kubectl apply -f argocd.yaml
```
**Clone the repo:**
```
git clone git@github.com:AhmedAmrgg/simple-gitops.git
```
**Adding infra tools:**
We will create new app for infra tools on apps directory:
```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: infra
  namespace: argocd   
spec:
  project: default
  source:
    repoURL: 'git@github.com:AhmedAmrgg/simple-gitops.git'   
    targetRevision: main
    path: infrastructure 

  destination:
    server: https://kubernetes.default.svc
    namespace: default

  syncPolicy:
    automated:   
      prune: true
      selfHeal: true
```
We will create infrastructure directory and add application for different infra tools like ingress nginx, kube-prometheus stack and loki.

**Add new application for our apps:**
We will create new app for application on apps directory:
```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: app
  namespace: argocd   
  annotations:
    argocd-image-updater.argoproj.io/image-list: myapp=docker.io/ahmedamr309/simple-trivy-app
    argocd-image-updater.argoproj.io/myapp.update-strategy: latest
    argocd-image-updater.argoproj.io/myapp.allow-tags: regexp:^v[0-9]+\.[0-9]+\.[0-9]+$
spec:
  project: default
  source:
    repoURL: 'git@github.com:AhmedAmrgg/simple-gitops.git'   
    targetRevision: main
    path: app-1   

  destination:
    server: https://kubernetes.default.svc
    namespace: default

  syncPolicy:
    automated:   
      prune: true
      selfHeal: true
```
## 🛠️ Infrastructure Tools

This project uses several cloud-native infrastructure tools to provide observability, networking, and automation:
- Ingress NGINX
Acts as the entry point to the Kubernetes cluster. It routes external HTTP/HTTPS traffic to the correct services inside the cluster using Ingress rules.
- kube-prometheus-stack
A monitoring stack that includes:
- Prometheus → Collects metrics from Kubernetes components, pods, and applications.
- Grafana → Provides dashboards to visualize metrics and logs.
- Alertmanager → Sends alerts when metrics cross thresholds.
- Loki
A log aggregation system (similar to Prometheus but for logs). It stores logs efficiently and integrates directly with Grafana.
- Promtail (part of Loki stack)
An agent that runs on Kubernetes nodes, collects logs from pods, and forwards them to Loki.
- Reloader
Watches ConfigMaps and Secrets. If they change, Reloader automatically restarts the affected pods so the updates are applied without manual intervention.

## 📊 Monitoring & Logging
- 🔹 Monitoring (kube-prometheus-stack)
- Prometheus scrapes metrics from Kubernetes components (kubelet, API server, etc.), system services, and application pods via exporters.
- The metrics are stored as time-series data in Prometheus.
- Grafana connects to Prometheus as a data source and provides visual dashboards (CPU, memory, pod health, cluster state, etc.).
- Alertmanager sends alerts (e.g., Slack, email, PagerDuty) when thresholds are exceeded (e.g., high CPU, pod crash loops).

## 🔹 Logging (Loki + Promtail)
- Promtail runs as a DaemonSet on each node. It reads container logs from /var/log/containers/ or Kubernetes API.
- Promtail attaches metadata (like pod, namespace, container labels) to each log.
- Logs are forwarded to Loki, which stores them in a lightweight, index-free way (cheaper and simpler than Elasticsearch).
- Grafana is connected to Loki, allowing developers to query and visualize logs alongside metrics.# GitOps-EKS
