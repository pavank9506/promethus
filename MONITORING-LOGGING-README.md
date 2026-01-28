## 1. Deploy Prometheus & Grafana (kube-prometheus-stack)

### Add Helm Repositories
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

### Create Monitoring Namespace
```
kubectl create namespace monitoring
```

### Install kube-prometheus-stack
```
helm install kube-prometheus-stack prometheus-community/kube-prometheus-stack --namespace monitoring
```

### Access Grafana and Prometheus
```
kubectl get svc -n monitoring
kubectl port-forward svc/kube-prometheus-stack-grafana 3000:80 -n monitoring
kubectl port-forward svc/kube-prometheus-stack-prometheus 9090:9090 -n monitoring
```
- Grafana: http://localhost:3000 (default login: admin/prom-operator)
- Prometheus: http://localhost:9090

---

# kube-prometheus-stack 

kube-prometheus-stack is a production-ready monitoring bundle for Kubernetes, delivered as a Helm chart, that installs everything you need for full cluster monitoring in one go.

> Think of it as:
>
> **“Prometheus + Alerting + Dashboards + Kubernetes metrics — all wired together.”**

## What kube-prometheus-stack Includes

| Component                | Purpose                                 |
|--------------------------|-----------------------------------------|
| Prometheus               | Collects & stores metrics               |
| Alertmanager             | Sends alerts (Slack, Email, PagerDuty)  |
| Grafana                  | Dashboards & visualization              |
| node-exporter            | Node CPU, memory, disk metrics          |
| kube-state-metrics       | Kubernetes object metrics               |
| ServiceMonitor/PodMonitor| Auto discovery of targets               |
| Prebuilt Dashboards      | Cluster & app insights                  |
| Predefined Alerts        | Node down, pod crash, etc               |

## Why kube-prometheus-stack Exists

Installing Prometheus manually is painful:
- Write scrape configs
- Create RBAC
- Set up exporters
- Build dashboards
- Configure alerts

👉 **kube-prometheus-stack solves all of this.**

## Architecture (Simple View)

```
Kubernetes Cluster
└── monitoring namespace
    ├── Prometheus (StatefulSet)
    ├── Alertmanager (StatefulSet)
    ├── Grafana (Deployment)
    ├── node-exporter (DaemonSet)
    ├── kube-state-metrics (Deployment)
    └── CRDs (ServiceMonitor, PodMonitor)
```

## How It Works
- Prometheus runs as a Pod
- node-exporter runs on every node
- kube-state-metrics exposes cluster state
- Prometheus scrapes metrics automatically
- Grafana shows dashboards
- Alertmanager sends alerts
- **All auto-configured by the chart.**

## Key CRDs Introduced
These are very important 👇

- **ServiceMonitor**
  - Monitors Kubernetes Services
  - `kind: ServiceMonitor`
- **PodMonitor**
  - Monitors Pods directly
  - `kind: PodMonitor`
- **PrometheusRule**
  - Defines alert & recording rules
  - `kind: PrometheusRule`

## Why Enterprises Use kube-prometheus-stack
- ✔ Kubernetes-native
- ✔ Battle-tested
- ✔ Easy upgrades
- ✔ GitOps-friendly
- ✔ Works with Argo CD
- ✔ Supports HA & scaling

## kube-prometheus-stack vs prometheus Chart

| Feature             | kube-prometheus-stack | prometheus |
|---------------------|:--------------------:|:----------:|
| Grafana             |          ✅           |     ❌     |
| Alertmanager        |          ✅           |     ✅     |
| Dashboards          |          ✅           |     ❌     |
| Kubernetes metrics  |          ✅           |  Partial   |
| CRDs                |          ✅           |     ❌     |
| Production ready    |          ✅           |     ❌     |

## Typical Use Cases
- Cluster monitoring
- Application monitoring
- SRE alerting
- DevOps observability
- Platform teams
