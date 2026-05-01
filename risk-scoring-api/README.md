# 📊 Real-Time Risk Scoring API

> Low-latency ML inference service that scores reinsurance policy applications in real time using Gradient Boosting models, served via a Spring Boot REST API.

![Python](https://img.shields.io/badge/Python-3.10-blue?style=flat-square&logo=python)
![PyTorch](https://img.shields.io/badge/PyTorch-2.x-EE4C2C?style=flat-square&logo=pytorch)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.x-brightgreen?style=flat-square&logo=spring)
![AWS Lambda](https://img.shields.io/badge/AWS-Lambda-FF9900?style=flat-square&logo=amazonaws)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-blue?style=flat-square&logo=postgresql)

---

## 📌 Overview

Reinsurance underwriters previously spent hours manually evaluating each policy application against risk tables and historical claims data. This system automates that process — a machine learning model trained on historical policy and claims data assigns a risk score (0–100) to each application in milliseconds, with a confidence interval and key risk factors explained via SHAP values.

The model is served through a Spring Boot REST API, deployed on AWS Lambda for auto-scaling, and integrated directly into the underwriting portal frontend.

---

## 🚀 Impact

| Metric | Before | After |
|---|---|---|
| Underwriting decision time | ~4 hours | ~8 minutes |
| Risk assessment accuracy (AUC) | 79% (manual rules) | 91.4% (ML model) |
| Policies processed/day | ~200 | 2,000+ |
| Underwriter cognitive load | High | Low (AI-assisted) |

---

## 🏗️ Architecture

```
┌──────────────────────────────┐
│   Underwriting Portal (React) │
└──────────────┬───────────────┘
               │ POST /api/score
               ▼
┌──────────────────────────────┐
│   Spring Boot REST API        │
│   (Input validation, logging) │
└──────────────┬───────────────┘
               │
       ┌───────▼────────┐
       │  Python ML     │
       │  Inference     │  (Gradient Boosting + SHAP)
       │  Service       │
       └───────┬────────┘
               │
       ┌───────▼────────┐
       │   Model Store  │  (AWS S3 — versioned .pkl files)
       └───────┬────────┘
               │
       ┌───────▼────────┐
       │   PostgreSQL   │  (Score history, audit trail)
       └────────────────┘
```

---

## 🤖 Model Details

| Aspect | Detail |
|---|---|
| Algorithm | Gradient Boosting (XGBoost) + Neural Net ensemble |
| Training Data | 5 years of historical policy + claims data |
| Features | 47 input features (age, policy type, claims history, region, etc.) |
| Target Variable | Binary: high-risk / low-risk + probability score 0–100 |
| AUC-ROC | 91.4% |
| Precision / Recall | 88.2% / 87.6% |
| Explainability | SHAP values per prediction |
| Retraining | Monthly automated pipeline |

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| ML Training | Python 3.10, XGBoost, scikit-learn, PyTorch |
| Explainability | SHAP |
| Model Serving | Python FastAPI (internal) + Spring Boot (external API) |
| Cloud Execution | AWS Lambda (auto-scale inference) |
| Model Storage | AWS S3 (versioned model artifacts) |
| Database | PostgreSQL (scores, audit, model versions) |
| Containerization | Docker |
| Monitoring | AWS CloudWatch, custom drift detection |

---

## 📁 Project Structure

```
risk-scoring-api/
├── ml-pipeline/
│   ├── data/
│   │   └── preprocessing.py      # Feature engineering
│   ├── training/
│   │   ├── train.py               # Model training + cross-validation
│   │   ├── evaluate.py            # AUC, precision, recall, SHAP
│   │   └── register.py            # Push model to S3
│   ├── serving/
│   │   └── inference.py           # FastAPI inference service
│   ├── tests/
│   └── requirements.txt
├── spring-api/
│   ├── src/main/java/
│   │   ├── controller/
│   │   │   └── RiskScoreController.java
│   │   ├── service/
│   │   │   └── RiskScoreService.java
│   │   ├── model/
│   │   │   ├── PolicyApplication.java
│   │   │   └── RiskScore.java
│   │   └── config/
│   ├── pom.xml
│   └── Dockerfile
├── lambda/
│   └── handler.py                 # AWS Lambda wrapper
├── docker-compose.yml
└── README.md
```

---

## ⚙️ Setup & Run

### Prerequisites
- Python 3.10+
- Java 17+
- Docker
- AWS credentials (for S3 model access)

### 1. Clone the repo

```bash
git clone https://github.com/MeenakshiGurrala/Projects.git
cd Projects/risk-scoring-api
```

### 2. Train and register the model

```bash
cd ml-pipeline
pip install -r requirements.txt
python training/train.py --data data/policies.csv
python training/register.py --model-path models/best_model.pkl
```

### 3. Start the ML inference service

```bash
cd ml-pipeline/serving
uvicorn inference:app --reload --port 5000
```

### 4. Start the Spring Boot API

```bash
cd spring-api
./mvnw spring-boot:run
# Runs on http://localhost:8080
```

### 5. Or use Docker Compose

```bash
docker-compose up --build
```

---

## 🔌 API Reference

### Score a policy application

```http
POST /api/v1/risk/score
Content-Type: application/json

{
  "applicantAge": 45,
  "policyType": "LIFE",
  "coverageAmount": 500000,
  "region": "MIDWEST",
  "priorClaims": 1,
  "healthScore": 72,
  ...
}
```

**Response:**

```json
{
  "applicationId": "POL-2025-00847",
  "riskScore": 68.4,
  "riskBand": "MEDIUM",
  "confidence": 0.91,
  "recommendation": "REVIEW",
  "shapExplanation": {
    "priorClaims": +12.3,
    "coverageAmount": +8.1,
    "applicantAge": -3.2
  },
  "processedAt": "2025-04-30T10:22:14Z"
}
```

### Get score history

```http
GET /api/v1/risk/history/{applicationId}
```

---

## 🧪 Testing

```bash
# ML pipeline tests
cd ml-pipeline && pytest tests/ -v

# Spring API tests
cd spring-api && ./mvnw test

# Integration test (end-to-end)
./scripts/integration-test.sh
```

---

## 👩‍💻 Author

**Meenakshi Gurrala** — Java Full Stack Developer | AI/ML Integrations  
📧 gurralameenakshi99@gmail.com  
🔗 [LinkedIn](https://www.linkedin.com/in/meenakshi-gurrala-0bb6a5301/) · [GitHub](https://github.com/MeenakshiGurrala)
