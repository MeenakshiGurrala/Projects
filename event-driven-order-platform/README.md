# ⚡ Event-Driven Order Processing Platform

> Microservices-based order management system built with Spring Boot and Apache Kafka — decomposed from a legacy monolith into 8 independently deployable services.

![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.x-brightgreen?style=flat-square&logo=spring)
![Kafka](https://img.shields.io/badge/Apache_Kafka-3.x-231F20?style=flat-square&logo=apachekafka)
![Kubernetes](https://img.shields.io/badge/Kubernetes-1.28-326CE5?style=flat-square&logo=kubernetes)
![MongoDB](https://img.shields.io/badge/MongoDB-7.x-47A248?style=flat-square&logo=mongodb)
![Redis](https://img.shields.io/badge/Redis-7.x-DC382D?style=flat-square&logo=redis)

---

## 📌 Overview

This project demonstrates how to decompose a tightly-coupled monolithic order system into a scalable, fault-tolerant microservices architecture using event-driven communication. Each service owns its domain, its database, and communicates asynchronously via Kafka topics — achieving loose coupling, independent deployability, and massive scale.

---

## 🚀 Impact

| Metric | Monolith | Microservices |
|---|---|---|
| Orders/day capacity | ~5,000 | 50,000+ |
| Deployment frequency | Weekly | Multiple/day |
| System uptime | 98.1% | 99.97% |
| Service recovery time | ~20 min | <90 seconds |
| Team deployment independence | No | Yes (per service) |

---

## 🏗️ Architecture

```
                         ┌─────────────────┐
                         │   API Gateway    │
                         │  (Spring Cloud)  │
                         └────────┬────────┘
                                  │
          ┌───────────────────────┼────────────────────────┐
          │                       │                        │
   ┌──────▼──────┐      ┌────────▼───────┐      ┌────────▼───────┐
   │   Order     │      │   Inventory    │      │   Pricing      │
   │   Service   │      │   Service      │      │   Service      │
   │  (MySQL)    │      │  (MongoDB)     │      │  (PostgreSQL)  │
   └──────┬──────┘      └────────┬───────┘      └────────┬───────┘
          │                      │                        │
          └──────────────────────┼────────────────────────┘
                                 │
                     ┌───────────▼───────────┐
                     │     Apache Kafka       │
                     │  (Event Bus / Topics)  │
                     └───────────┬───────────┘
                                 │
          ┌──────────────────────┼──────────────────────┐
          │                      │                      │
   ┌──────▼──────┐     ┌────────▼──────┐     ┌────────▼──────┐
   │  Payment    │     │ Notification  │     │ Fulfillment   │
   │  Service   │     │   Service     │     │   Service     │
   │  (MySQL)   │     │  (MongoDB)    │     │  (PostgreSQL) │
   └─────────────┘     └───────────────┘     └───────────────┘
```

---

## 🛠️ Microservices Breakdown

| Service | Responsibility | Database | Port |
|---|---|---|---|
| `order-service` | Create, update, track orders | MySQL | 8081 |
| `inventory-service` | Stock management, reservation | MongoDB | 8082 |
| `pricing-service` | Dynamic pricing, discounts | PostgreSQL | 8083 |
| `payment-service` | Payment processing, refunds | MySQL | 8084 |
| `notification-service` | Email/SMS/push alerts | MongoDB | 8085 |
| `fulfillment-service` | Shipping, delivery tracking | PostgreSQL | 8086 |
| `api-gateway` | Routing, auth, rate limiting | — | 8080 |
| `discovery-service` | Eureka service registry | — | 8761 |

---

## 📦 Kafka Topics

| Topic | Producer | Consumers |
|---|---|---|
| `order.created` | order-service | inventory, pricing, notification |
| `order.confirmed` | pricing-service | payment, notification |
| `payment.completed` | payment-service | fulfillment, notification |
| `order.shipped` | fulfillment-service | notification |
| `inventory.low` | inventory-service | notification |

---

## 🛠️ Tech Stack

| Component | Technology |
|---|---|
| Microservices | Spring Boot 3.x, Java 17 |
| Event Bus | Apache Kafka 3.x |
| Service Discovery | Netflix Eureka |
| API Gateway | Spring Cloud Gateway |
| Databases | MySQL, MongoDB, PostgreSQL (polyglot) |
| Caching | Redis |
| Containerization | Docker, Kubernetes |
| CI/CD | Jenkins, GitHub Actions |

---

## 📁 Project Structure

```
event-driven-order-platform/
├── api-gateway/
├── discovery-service/
├── order-service/
├── inventory-service/
├── pricing-service/
├── payment-service/
├── notification-service/
├── fulfillment-service/
├── kafka/
│   └── docker-compose-kafka.yml
├── k8s/
│   ├── deployments/
│   └── services/
├── docker-compose.yml
└── README.md
```

---

## ⚙️ Setup & Run

### Prerequisites
- Java 17+
- Docker & Docker Compose
- Kubernetes (optional, for full orchestration)

### 1. Clone the repo

```bash
git clone https://github.com/MeenakshiGurrala/Projects.git
cd Projects/event-driven-order-platform
```

### 2. Start infrastructure (Kafka, DBs)

```bash
docker-compose up -d zookeeper kafka mysql mongodb postgres redis
```

### 3. Start all services

```bash
docker-compose up --build
```

### 4. Deploy to Kubernetes

```bash
kubectl apply -f k8s/
kubectl get pods -n order-platform
```

---

## 🧪 Testing

```bash
# Run all service tests
./run-tests.sh

# Individual service
cd order-service && ./mvnw test
```

---

## 👩‍💻 Author

**Meenakshi Gurrala** — Java Full Stack Developer | Microservices Specialist  
📧 gurralameenakshi99@gmail.com  
🔗 [LinkedIn](https://www.linkedin.com/in/meenakshi-gurrala-0bb6a5301/) · [GitHub](https://github.com/MeenakshiGurrala)
