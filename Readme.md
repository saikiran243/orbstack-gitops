# EKS MLOps Platform (GitOps-Driven)

This repository contains the infrastructure-as-code for a scalable, self-healing MLOps platform deployed on **AWS EKS**. The platform utilizes a GitOps methodology to maintain cluster state, integrated with Datadog for enterprise-grade observability.

## 🚀 Architecture Overview
- **GitOps Engine:** [Argo CD](https://argo-cd.readthedocs.io/) continuously syncs this repository with the EKS cluster state.
- **Service Mesh:** [Istio](https://istio.io/) provides mTLS security and internal traffic management.
- **API Gateway:** [Kong](https://konghq.com/) manages traffic ingress and rate limiting.
- **Observability:** [Datadog](https://www.datadoghq.com/) provides full-stack monitoring (metrics, logs, traces) with Kubernetes Autodiscovery.
- **Compute:** [AWS EKS Auto Mode](https://docs.aws.amazon.com/eks/latest/userguide/eks-auto-mode.html) for dynamic, minimal-compute scaling.

## 🛠 Platform Capabilities

### 1. Declarative Infrastructure (GitOps)
The entire cluster configuration is version-controlled. Argo CD ensures that any change pushed to this repo is automatically applied to EKS, eliminating configuration drift.

### 2. Automated Self-Healing
Every application workload is configured with **Liveness Probes**. 
* **Failure Scenario:** If an inference container exhausts memory (OOMKilled) or deadlocks, Kubernetes detects the health failure and automatically terminates and recreates the pod within seconds.
* **Resilience:** This zero-touch recovery ensures high availability for mission-critical computer vision pipelines.

### 3. Enterprise Observability
Datadog integration leverages **Unified Service Tagging**. By using Kubernetes annotations, all services are automatically identified in Datadog, allowing for:
* Real-time monitoring of CPU/Memory spikes during inference tasks.
* Automated alerting on Pod CrashLoops.
* Visual dependency mapping via Istio's sidecar proxies.

## 📂 Repository Structure
```text
├── apps/               # Argo CD Application definitions
├── bootstrap/          # Enterprise bootstrap configuration
├── workloads/          # Application manifests (Dev/Stage/Prod overlays)
└── README.md