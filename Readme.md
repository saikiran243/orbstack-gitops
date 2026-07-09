# EKS MLOps Platform (GitOps-Driven)

This repository contains the infrastructure-as-code for a scalable, self-healing MLOps platform deployed on **AWS EKS**.

## 🚀 Architecture Overview
- **GitOps Engine:** [Argo CD](https://argo-cd.readthedocs.io/) continuously syncs this repository with the EKS cluster state.
- **Service Mesh:** [Istio](https://istio.io/) provides mTLS security and internal traffic management via Envoy sidecars.
- **API Gateway:** [Kong](https://konghq.com/) manages traffic ingress and rate limiting.
- **Observability:** [Datadog](https://www.datadoghq.com/) provides full-stack monitoring (metrics, logs, traces) with Kubernetes Autodiscovery.
- **Compute:** [AWS EKS Auto Mode](https://docs.aws.amazon.com/eks/latest/userguide/eks-auto-mode.html) for dynamic, minimal-compute scaling.

## ⚙️ Platform Functionality

### 1. Zero-Touch Infrastructure (GitOps)
- **Continuous Reconciliation:** Any change committed to this repository is automatically synchronized to the EKS cluster by Argo CD.
- **Drift Detection:** Argo CD monitors the live state of the cluster; if a manual configuration change occurs, the platform automatically "self-heals" back to the defined Git state.

### 2. Intelligent Traffic & Security (Istio)
- **Zero Trust Networking:** Istio enforces mTLS (Mutual TLS) on all internal service-to-service communication, ensuring traffic is encrypted and verified.
- **Canary Deployments:** Traffic routing can be weighted (e.g., 95/5) to allow for safe testing of new application versions in production.
- **Automated Observability:** Envoy proxies automatically capture request latency, error rates, and traffic volume, exporting this data to Datadog without requiring application code changes.

### 3. High Availability & Self-Healing
- **Failure Recovery:** Every workload utilizes **Liveness Probes**. If a process deadlocks or hits a memory limit, Kubernetes automatically kills the unhealthy pod and provisions a fresh replacement.
- **Scaling:** EKS Auto Mode observes workload resource requests (`cpu`/`memory`) and provisions the necessary compute capacity (down to micro-instances) on-demand.

### 4. Enterprise Observability (Datadog)
- **Unified Service Tagging:** Standardized tags (`env`, `service`, `version`) allow for instant cross-service analysis.
- **Proactive Alerting:** Datadog tracks pod lifecycle events, alerting engineers on crash loops or abnormal resource consumption trends before they impact the user experience.

---

## 🛠 Setup & Deployment
1. **Bootstrap Argo CD:**
   `kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml`

2. **Configure Datadog:**
   `kubectl create secret generic datadog-secret --from-literal=api-key="<YOUR_KEY>" -n datadog`

3. **Trigger GitOps Sync:**
   `kubectl apply -f bootstrap/enterprise-bootstrap.yaml`