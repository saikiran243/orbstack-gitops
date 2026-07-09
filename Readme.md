# EKS MLOps Platform (GitOps-Driven)

This repository contains the infrastructure-as-code for a scalable, self-healing MLOps platform deployed on **AWS EKS**.

## 🚀 Architecture Overview
- **GitOps Engine:** Argo CD (State management).
- **Service Mesh:** Istio (mTLS & Traffic routing).
- **API Gateway:** Kong (Rate limiting & Proxying).
- **Observability:** Datadog (Metrics, Logs, & Automated Traces).
- **Compute:** AWS EKS Auto Mode (On-demand resource scaling).

## ⚙️ Platform Functionality

### 1. Multi-Environment Strategy (Kustomize)
We utilize **Kustomize** to manage `dev` and `stage` environments:
- **Base Layer:** Standard application manifests.
- **Overlays:** Environment-specific patches (e.g., replica counts, CPU/Memory limits, ingress rules).
- **Benefit:** DRY approach allows for rapid, consistent environment promotion.

### 2. Zero-Touch Infrastructure (GitOps)
- **Continuous Reconciliation:** Any change to this repo is auto-synced to EKS.
- **Drift Detection:** Argo CD monitors the live state, self-healing if manual configuration changes are detected.

### 3. Intelligent Traffic & Security (Istio)
- **Zero Trust Networking:** mTLS enforcement for all service communication.
- **Automated Observability:** Sidecar proxies capture latency/traffic volume, streaming directly to Datadog.

### 4. High Availability & Self-Healing
- **Failure Recovery:** Liveness Probes detect application deadlocks; Kubernetes replaces failed pods instantly.
- **Scaling:** EKS Auto Mode provisions compute capacity on-demand.

### 5. Disaster Recovery & Portability
- **Environment Parity:** Because we use GitOps and Kustomize, we can rebuild the entire production environment in a new EKS cluster from scratch in under 10 minutes.
- **Portability:** Our stack is cloud-agnostic; while currently running on EKS, the manifests are compatible with any CNCF-compliant Kubernetes distribution.

---

## 🏗 Setup & Deployment
1. **Bootstrap Argo CD:**
   `kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml`
2. **Configure Datadog:**
   `kubectl create secret generic datadog-secret --from-literal=api-key="<YOUR_KEY>" -n datadog`
3. **Trigger GitOps Sync:**
   `kubectl apply -f bootstrap/enterprise-bootstrap.yaml`