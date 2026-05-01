# 🚀 Zero-Downtime CI/CD Platform on AWS

> Fully automated CI/CD pipeline for a 12-service microservices application — code to production in under 8 minutes, with blue-green deployments, automated rollback, and full observability.

![AWS EKS](https://img.shields.io/badge/AWS-EKS-FF9900?style=flat-square&logo=amazonaws)
![Terraform](https://img.shields.io/badge/Terraform-1.6-7B42BC?style=flat-square&logo=terraform)
![Kubernetes](https://img.shields.io/badge/Kubernetes-1.28-326CE5?style=flat-square&logo=kubernetes)
![Jenkins](https://img.shields.io/badge/Jenkins-2.x-D24939?style=flat-square&logo=jenkins)
![Prometheus](https://img.shields.io/badge/Prometheus-2.x-E6522C?style=flat-square&logo=prometheus)

---

## 📌 Overview

This project delivers a production-ready DevOps platform that automates the entire software delivery lifecycle — from code commit through build, test, containerization, staging, and production release — for a 12-service microservices application on AWS EKS.

Key highlights: blue-green deployments for zero downtime, automated rollback on health check failure, infrastructure-as-code with Terraform, and a full Prometheus + Grafana observability stack.

---

## 🚀 Impact

| Metric | Before | After |
|---|---|---|
| Release cycle | 2 weeks | Same-day |
| Deployment time | ~4 hours (manual) | ~8 minutes (automated) |
| Deployment failures | ~15% | <2% |
| Manual deployment steps | 40+ | 0 |
| Mean time to recovery | ~45 min | <5 min (auto-rollback) |

---

## 🏗️ Pipeline Architecture

```
Developer Push
      │
      ▼
┌─────────────┐
│   GitHub    │  (PR trigger / merge to main)
└──────┬──────┘
       │ Webhook
       ▼
┌─────────────┐
│   Jenkins   │  Stage 1: Checkout + Build (Maven/Gradle)
│   Pipeline  │  Stage 2: Unit & Integration Tests (JUnit)
│             │  Stage 3: SonarQube Code Quality Scan
│             │  Stage 4: Docker Build & Push (ECR)
│             │  Stage 5: Terraform Plan (infra changes)
│             │  Stage 6: Deploy to Staging (EKS)
│             │  Stage 7: Smoke Tests
│             │  Stage 8: Blue-Green Deploy to Production
└──────┬──────┘
       │
       ▼
┌─────────────┐        ┌──────────────────┐
│  AWS EKS    │◄───────│  Terraform (IaC)  │
│  Cluster    │        │  VPC, EKS, RDS    │
└──────┬──────┘        │  S3, IAM, ALB     │
       │               └──────────────────┘
       ▼
┌─────────────────────────────────┐
│  Prometheus + Grafana + Alertmgr │  (Observability)
└─────────────────────────────────┘
```

---

## 🛠️ Tech Stack

| Component | Technology |
|---|---|
| Cloud Provider | AWS (EKS, ECR, S3, RDS, ALB, IAM) |
| Infrastructure as Code | Terraform 1.6 |
| Container Orchestration | Kubernetes 1.28 (EKS) |
| CI/CD | Jenkins 2.x, GitHub Actions |
| Containerization | Docker |
| Deployment Strategy | Blue-Green, Rolling Updates |
| Monitoring | Prometheus, Grafana |
| Alerting | Alertmanager, PagerDuty |
| Secret Management | AWS Secrets Manager, Kubernetes Secrets |
| Code Quality | SonarQube |

---

## 📁 Project Structure

```
cicd-aws-platform/
├── terraform/
│   ├── modules/
│   │   ├── vpc/              # VPC, subnets, security groups
│   │   ├── eks/              # EKS cluster + node groups
│   │   ├── rds/              # Managed database
│   │   └── alb/              # Application load balancer
│   ├── environments/
│   │   ├── staging/
│   │   └── production/
│   └── main.tf
├── jenkins/
│   ├── Jenkinsfile           # Main pipeline definition
│   └── shared-library/       # Reusable pipeline steps
├── k8s/
│   ├── base/                 # Base Kubernetes manifests
│   ├── overlays/
│   │   ├── staging/
│   │   └── production/
│   └── blue-green/           # Blue-green deployment configs
├── monitoring/
│   ├── prometheus/
│   │   └── prometheus.yml
│   ├── grafana/
│   │   └── dashboards/
│   └── alertmanager/
│       └── alerts.yml
├── scripts/
│   ├── deploy.sh
│   ├── rollback.sh
│   └── health-check.sh
└── README.md
```

---

## ⚙️ Setup & Deploy

### Prerequisites
- AWS CLI configured with appropriate IAM roles
- Terraform 1.6+
- kubectl
- Jenkins instance (or use the included Docker setup)

### 1. Clone the repo

```bash
git clone https://github.com/MeenakshiGurrala/Projects.git
cd Projects/cicd-aws-platform
```

### 2. Provision infrastructure with Terraform

```bash
cd terraform/environments/production
terraform init
terraform plan
terraform apply
```

### 3. Configure Jenkins

```bash
# Import the Jenkinsfile and configure credentials:
# - AWS_ACCESS_KEY_ID / AWS_SECRET_ACCESS_KEY
# - ECR registry URL
# - SonarQube token
# - Kubernetes kubeconfig
```

### 4. Deploy monitoring stack

```bash
kubectl apply -f monitoring/
kubectl port-forward svc/grafana 3000:3000 -n monitoring
# Access Grafana at http://localhost:3000
```

### 5. Trigger a deployment

```bash
# Push to main branch — pipeline triggers automatically
git push origin main
```

### Manual rollback

```bash
./scripts/rollback.sh --service order-service --version v1.2.3
```

---

## 📊 Grafana Dashboards

- **Cluster Overview** — Node CPU/memory, pod health, network I/O
- **Service Metrics** — Request rate, error rate, latency (RED metrics)
- **Deployment Tracker** — Live deploy status, version per service
- **Kafka Metrics** — Consumer lag, throughput, topic health
- **JVM Metrics** — Heap usage, GC pauses, thread count

---

## 🔐 Security

- IAM roles with least-privilege access (IRSA for pod-level AWS access)
- Secrets stored in AWS Secrets Manager, injected at runtime
- Network policies to restrict pod-to-pod communication
- Container image scanning with AWS ECR scan on push
- RBAC enforced at Kubernetes namespace level

---

## 🧪 Testing the Pipeline

```bash
# Trigger a test deployment (staging)
./scripts/deploy.sh --env staging --service order-service --tag latest

# Verify health
./scripts/health-check.sh --env staging
```

---

## 👩‍💻 Author

**Meenakshi Gurrala** — Java Full Stack Developer | Cloud & DevOps  
📧 gurralameenakshi99@gmail.com  
🔗 [LinkedIn](https://www.linkedin.com/in/meenakshi-gurrala-0bb6a5301/) · [GitHub](https://github.com/MeenakshiGurrala)
