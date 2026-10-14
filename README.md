# NexusML — Enterprise End-to-End MLOps, LLMOps & Agentic AI Platform

[![Platform Version](https://img.shields.io/badge/version-3.4.0--enterprise-blue.svg)](https://github.com/nexusml/enterprise-ai-platform)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-EKS_v1.29-326CE5.svg)](https://kubernetes.io/)
[![AWS](https://img.shields.io/badge/AWS-EKS_|_ECR_|_RDS_|_S3-232F3E.svg)](https://aws.amazon.com/)
[![License](https://img.shields.io/badge/license-Apache--2.0-green.svg)](LICENSE)

NexusML is a production-grade, multi-tenant enterprise MLOps, LLMOps, and Agentic AI Platform engineered to FAANG standards. It automates the entire AI lifecycle from data ingestion, profiling, feature store management, AutoML, distributed GPU training, model registry, Triton inference serving, Kubernetes autoscaling, Evidently AI drift detection, auto-retraining, and LLM prompt evaluation.

---

## 🏗️ High-Level System Architecture

```
                                 ┌──────────────────────────────────────────────┐
                                 │       NexusML Enterprise Web UI & API Gateway│
                                 │     (React 19 + TypeScript + Express + Vite) │
                                 └──────────────────────┬───────────────────────┘
                                                        │
                      ┌─────────────────────────────────┴────────────────────────────────┐
                      │                                                                  │
       ┌──────────────▼──────────────┐                                    ┌──────────────▼──────────────┐
       │   Agentic AI Orchestrator   │                                    │    Feature Store & Storage  │
       │    (LangGraph + Gemini)     │                                    │  (Feast + S3 + PostgreSQL)  │
       └──────────────┬──────────────┘                                    └──────────────┬──────────────┘
                      │                                                                  │
                      ├─────────────────────────────────┬────────────────────────────────┤
                      │                                 │                                │
       ┌──────────────▼──────────────┐   ┌──────────────▼──────────────┐   ┌──────────────▼──────────────┐
       │   Distributed GPU Training  │   │     MLflow Model Registry   │   │   Triton Inference Serving  │
       │  (Optuna + XGBoost/PyTorch) │   │  (Artifacts + Lineage DB)   │   │ (Canary / Blue-Green Envoy) │
       └──────────────┬──────────────┘   └──────────────┬──────────────┘   └──────────────┬──────────────┘
                      │                                 │                                │
                      └─────────────────────────────────┼────────────────────────────────┘
                                                        │
                                         ┌──────────────▼──────────────┐
                                         │  AI Observability & Drift   │
                                         │  (Evidently AI + Prometheus)│
                                         └─────────────────────────────┘
```

---

## ⚡ Enterprise Capabilities & Modules

### 1. 🤖 Agentic AI Workflow Engine (LangGraph)
Autonomous multi-agent architecture executing sequential and parallel MLOps pipelines:
1. **Data Validation Agent**: Computes schemas, null-rate bounds, duplicate rates, and data quality scores.
2. **EDA Agent**: Generates correlation matrices, feature distributions, skewness reports, and anomaly graphs.
3. **Feature Engineering Agent**: Encodes categorical values, scales numeric columns, and syncs Feast offline/online stores.
4. **Training Agent**: Launches hyperparameter search spaces using Optuna across distributed GPU workers.
5. **Evaluation Agent**: Computes SHAP TreeExplainer importances, LIME local predictions, and fairness metrics.
6. **Model Registry Agent**: Tagging, versioning, and promoting candidate models to MLflow staging/production.
7. **Deployment Agent**: Triggers automated zero-downtime Kubernetes deployments (10% Canary split -> 100% Prod).
8. **Drift Monitor Agent**: Continuously audits Kolmogorov-Smirnov (KS-test) and Population Stability Index (PSI).

### 2. 🗄️ Feast Feature Store (Offline & Online)
- Point-in-time correct historical joins preventing data leakage.
- Low-latency Redis online store for real-time inference feature serving (<5ms latency).
- S3 / Parquet offline feature retrieval for training pipelines.

### 3. 🧪 LLMOps & Prompt Engineering Studio
- **Prompt Registry & Versioning**: Complete audit trail of prompt templates with variable injection.
- **RAGAS & DeepEval Metrics**: Automated evaluation of Faithfulness, Answer Relevance, and Hallucination rates.
- **Token & Cost Analytics**: Real-time token consumption tracking per tenant and LLM model provider.

### 4. 🛰️ Inference Gateway & Traffic Management
- **High-Performance Serving**: Triton Inference Server (gRPC & REST APIs) with GPU acceleration.
- **Traffic Routing**: Automated 10/90 Canary deployment rollout with immediate rollback on error spikes (>0.1%).

### 5. 📊 AI Observability & Auto-Retraining
- **Evidently AI Integration**: Live monitoring of feature drift, target drift, and confidence degradation.
- **Auto-Retrain Triggers**: Automated GitHub Actions dispatch whenever PSI exceeds 0.25 threshold.

---

## 📦 Directory & Repository Structure

```
├── .github/workflows/
│   ├── ci-cd.yml                # Main CI/CD GitHub Actions Workflow
│   └── ml-retrain.yml           # Automated Model Retraining Trigger Workflow
├── k8s/
│   ├── deployment.yaml          # Kubernetes Deployment Manifests (Core API & Triton)
│   ├── service.yaml             # Kubernetes Services (ClusterIP & LoadBalancer)
│   ├── ingress.yaml             # NGINX Ingress Controller & TLS Cert Manager
│   └── hpa.yaml                 # Horizontal Pod Autoscalers (CPU & GPU metrics)
├── pipelines/
│   └── langgraph_orchestrator.py# Python Autonomous LangGraph Multi-Agent Engine
├── terraform/
│   ├── main.tf                  # AWS Infrastructure (EKS, ECR, S3, RDS, VPC)
│   └── variables.tf             # Terraform Input Variables & Credentials
├── monitoring/
│   ├── prometheus.yml           # Prometheus Scraping Configurations
│   └── evidently_config.yaml    # Evidently AI Drift Threshold Rules
├── src/                         # Enterprise React + TypeScript Web Application
│   ├── components/              # 16 Modular MLOps & LLMOps Feature Views
│   ├── data/                    # Deep Mock & Live Platform State Engines
│   └── types/                   # Complete TypeScript Interfaces & Schemas
├── Dockerfile                   # Multi-Stage Secure Container Definition
├── docker-compose.yml           # Full Local Stack Orchestration
├── server.ts                    # Enterprise Express API Server + Gemini Integration
├── metadata.json                # Application Metadata & AI Capabilities
└── package.json                 # Dependency Manifest & Scripts
```

---

## 🚀 Step-by-Step Local Setup & Deployment

### Prerequisites
- Node.js 20+
- Docker & Docker Compose
- Python 3.10+ (optional for CLI pipelines)

### 1. Local Development Mode
```bash
# Install dependencies
npm install

# Start development server with hot-reloading
npm run dev
```
Open [http://localhost:3000](http://localhost:3000) in your browser.

### 2. Run Full Platform Stack via Docker Compose
```bash
docker-compose up -d --build
```
Services exposed:
- **NexusML Web Application**: [http://localhost:3000](http://localhost:3000)
- **MLflow Tracking Dashboard**: [http://localhost:5000](http://localhost:5000)
- **MinIO S3 Storage Console**: [http://localhost:9001](http://localhost:9001)
- **Prometheus Metrics**: [http://localhost:9090](http://localhost:9090)
- **Grafana Analytics**: [http://localhost:3001](http://localhost:3001)

---

## ☁️ Production Deployment on AWS EKS

### 1. Provision Infrastructure with Terraform
```bash
cd terraform
terraform init
terraform plan
terraform apply -auto-approve
```

### 2. Deploy Manifests to Kubernetes Cluster
```bash
# Update kubeconfig to connect to EKS
aws eks update-kubeconfig --name nexusml-prod-eks --region us-east-1

# Apply Kubernetes production manifests
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
kubectl apply -f k8s/ingress.yaml
kubectl apply -f k8s/hpa.yaml
```

---

## 🔐 Security & RBAC Standard

NexusML implements Role-Based Access Control (RBAC) and Attribute-Based Access Control (ABAC):
- **Admin**: Full read/write access across all tenants, infrastructure, and user permissions.
- **ML Engineer**: Pipeline execution, model deployment, feature store updates, and traffic routing.
- **Data Scientist**: Experimentation, model training, AutoML, dataset profiling, and SHAP auditing.
- **Business Viewer**: Read-only dashboards, metric reports, and cost analysis.

---

## 📄 License

This enterprise platform software is licensed under the Apache 2.0 License.
