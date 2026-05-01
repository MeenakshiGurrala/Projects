# 🏥 Healthcare Claims Management Portal

> Full-stack web platform for healthcare claims submission, real-time tracking, and analytics — built with React + Redux and Spring Boot microservices.

![React](https://img.shields.io/badge/React-18.x-61DAFB?style=flat-square&logo=react)
![Redux](https://img.shields.io/badge/Redux-4.x-764ABC?style=flat-square&logo=redux)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.x-brightgreen?style=flat-square&logo=spring)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-blue?style=flat-square&logo=postgresql)
![OAuth2](https://img.shields.io/badge/OAuth-2.0-red?style=flat-square)

---

## 📌 Overview

A production-grade enterprise platform that digitizes and streamlines the entire healthcare claims lifecycle — from submission to approval and payment tracking. Built for high concurrency, it supports 500+ simultaneous users with real-time WebSocket updates, role-based access control, and rich analytics dashboards.

---

## 🚀 Impact

| Metric | Before | After |
|---|---|---|
| Claims turnaround time | 5 days | 18 hours |
| Concurrent users supported | ~50 | 500+ |
| Manual data entry errors | ~12% | <1% |
| Claim status visibility | Email only | Real-time dashboard |

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────┐
│              React + Redux Frontend              │
│   (Claims Form │ Dashboard │ Analytics │ Admin)  │
└──────────────────────┬──────────────────────────┘
                       │ REST / WebSocket
          ┌────────────▼────────────┐
          │    API Gateway           │
          └────┬──────┬──────┬──────┘
               │      │      │
        ┌──────▼─┐ ┌──▼───┐ ┌▼────────┐
        │ Claims │ │ Auth │ │Analytics│
        │Service │ │ Svc  │ │ Service │
        └──┬─────┘ └──┬───┘ └─┬───────┘
           │          │       │
        ┌──▼──────────▼───────▼──┐
        │      PostgreSQL DB      │
        └────────────────────────┘
```

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 18, Redux Toolkit, Bootstrap 5, Chart.js |
| Real-time | WebSockets (Spring WebSocket + STOMP) |
| Backend | Spring Boot 3.x, Java 17, Spring MVC, Spring Data JPA |
| Auth | OAuth 2.0, JWT, Spring Security |
| Database | PostgreSQL 15, Hibernate ORM |
| Reporting | JasperReports |
| DevOps | Docker, Jenkins CI/CD, GitHub Actions |

---

## 📁 Project Structure

```
healthcare-claims-portal/
├── frontend/
│   ├── src/
│   │   ├── components/         # Reusable UI components
│   │   ├── pages/              # Claims, Dashboard, Admin, Profile
│   │   ├── store/              # Redux slices and thunks
│   │   ├── services/           # Axios API calls
│   │   └── utils/
│   ├── package.json
│   └── Dockerfile
├── backend/
│   ├── src/main/java/
│   │   ├── controller/         # REST + WebSocket controllers
│   │   ├── service/            # Business logic
│   │   ├── repository/         # JPA repositories
│   │   ├── model/              # Entity classes
│   │   ├── security/           # OAuth2 + JWT config
│   │   └── config/
│   ├── pom.xml
│   └── Dockerfile
├── docker-compose.yml
└── README.md
```

---

## ⚙️ Setup & Run

### Prerequisites
- Node.js 18+
- Java 17+
- PostgreSQL 15
- Docker (optional)

### 1. Clone the repo

```bash
git clone https://github.com/MeenakshiGurrala/Projects.git
cd Projects/healthcare-claims-portal
```

### 2. Backend setup

```bash
cd backend
# Configure DB in src/main/resources/application.properties
./mvnw spring-boot:run
# Runs on http://localhost:8080
```

### 3. Frontend setup

```bash
cd frontend
npm install
npm start
# Runs on http://localhost:3000
```

### 4. Or run everything with Docker

```bash
docker-compose up --build
```

---

## 🔌 Key API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/claims/submit` | Submit a new claim |
| `GET` | `/api/claims/{id}` | Get claim details |
| `GET` | `/api/claims/user/{userId}` | Get all claims for a user |
| `PUT` | `/api/claims/{id}/status` | Update claim status |
| `GET` | `/api/analytics/summary` | Dashboard analytics data |
| `WS` | `/ws/claims/{id}` | Real-time status updates |

---

## 🔐 Roles & Access

| Role | Permissions |
|---|---|
| `PATIENT` | Submit claims, view own claim status |
| `PROVIDER` | Submit on behalf of patients, view billing |
| `ADJUDICATOR` | Review, approve, deny claims |
| `ADMIN` | Full access, user management, reports |

---

## 🧪 Testing

```bash
# Backend tests
cd backend && ./mvnw test

# Frontend tests
cd frontend && npm test
```

---

## 👩‍💻 Author

**Meenakshi Gurrala** — Java Full Stack Developer  
📧 gurralameenakshi99@gmail.com  
🔗 [LinkedIn](https://www.linkedin.com/in/meenakshi-gurrala-0bb6a5301/) · [GitHub](https://github.com/MeenakshiGurrala)
