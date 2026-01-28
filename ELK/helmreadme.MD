# ELK Stack Installation on Kubernetes

This guide explains how to install the ELK stack (Elasticsearch, Logstash, Kibana) on a Kubernetes cluster using Helm.

## Prerequisites
- A running Kubernetes cluster
- Helm installed and configured

## Steps

### 1. Add Elastic Helm Repository
```
helm repo add elastic https://helm.elastic.co
helm repo update
```

### 2. Install Elasticsearch
```
helm install elasticsearch elastic/elasticsearch
```

### 3. Install Kibana
```
helm install kibana elastic/kibana
```

### 4. (Optional) Install Logstash
```
helm install logstash elastic/logstash
```

### 5. Verify Installation
```
kubectl get pods
kubectl get svc
```

## Custom Configuration
You can customize deployments using a `values.yaml` file. Refer to the [Elastic Helm Charts documentation](https://github.com/elastic/helm-charts) for advanced settings.

## Accessing Kibana
To access Kibana from outside the cluster, expose the service using NodePort or LoadBalancer, or use port-forwarding:
```
kubectl port-forward svc/kibana-kibana 5601:5601
```
Then open http://localhost:5601 in your browser.

---

## Installing Filebeat
Filebeat is a lightweight shipper for forwarding and centralizing log data. To deploy Filebeat on your Kubernetes cluster:

### 1. Install Filebeat using Helm
```
helm install filebeat elastic/filebeat
```

### 2. (Optional) Custom Configuration
You can customize Filebeat by creating a `values.yaml` file. For example, to specify log paths or output settings, refer to the [Filebeat Helm Chart documentation](https://github.com/elastic/helm-charts/tree/main/filebeat).

### 3. Verify Filebeat Deployment
```
kubectl get pods -l app=filebeat
```

### 4. Viewing Filebeat Logs
```
kubectl logs <filebeat-pod-name>
```

---

Filebeat will automatically collect logs from your Kubernetes nodes and forward them to Elasticsearch (or Logstash, if configured). For advanced configuration, see the official Elastic documentation.

## ELK Components and Kubernetes Resources

| Component     | Kubernetes Resource   |
| ------------- | --------------------- |
| Elasticsearch | StatefulSet           |
| Kibana        | Deployment            |
| Fluent Bit    | DaemonSet             |
| Logstash      | Deployment (optional) |

This table summarizes how each ELK component is typically deployed in Kubernetes using Helm charts.
