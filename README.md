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
<!-- ## 🏗️ Architecture -->
## Project Structure 
```.
├── .github/workflows/   # GitHub Actions pipelines
├── manifests/           # Kubernetes YAML manifests
│   ├── argocd/
│   ├── ingress-nginx/
│   ├── kube-prometheus-stack/
│   ├── loki-promtail/
│   └── reloader/
├── charts/              # Helm charts (if used)
└── README.md
```
## 🚀 Getting Started
**Clone the repo:**
```
git clone https://github.com/your-username/devops-gitops-argocd.git
```
**Deploy ArgoCD:** 
```
kubectl apply -f manifests/argocd/
```
Deploy monitoring & logging stack:
```
kubectl apply -f manifests/kube-prometheus-stack/
kubectl apply -f manifests/loki-promtail/
```
Expose applications via NGINX Ingress.
## 📊 Dashboards & Logs
**Grafana** → http://<ingress-url>/grafana
**Prometheus** → http://<ingress-url>/prometheus
**Loki** → Logs accessible via Grafana dashboards

## 🤝 Contributions
- Feel free to fork, open issues, or submit PRs to enhance the project!
- Would you like me to also make a LinkedIn post version of this README (short, professional, and attractive) so you can showcase the project to recruiters?
