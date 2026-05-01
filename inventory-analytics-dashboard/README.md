# 📦 Enterprise Inventory & Analytics Dashboard

> Real-time inventory tracking and analytics platform for multi-warehouse operations — built with Angular 12, Spring Boot, and DB2.

![Angular](https://img.shields.io/badge/Angular-12-DD0031?style=flat-square&logo=angular)
![TypeScript](https://img.shields.io/badge/TypeScript-4.x-3178C6?style=flat-square&logo=typescript)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.x-brightgreen?style=flat-square&logo=spring)
![DB2](https://img.shields.io/badge/IBM-DB2-052FAD?style=flat-square&logo=ibm)
![Chart.js](https://img.shields.io/badge/Chart.js-4.x-FF6384?style=flat-square)

---

## 📌 Overview

Large-scale distribution operations across multiple warehouses generate enormous amounts of inventory data — but without real-time visibility, managers can't act fast enough to prevent stockouts, overstock, or fulfillment delays. This dashboard provides real-time inventory tracking, predictive low-stock alerts, trend analytics, and drill-down reporting for 200+ warehouse managers across multiple locations.

---

## 🚀 Impact

| Metric | Before | After |
|---|---|---|
| Data freshness | Daily batch reports | Real-time (< 30s lag) |
| Stockout incidents | Baseline | Reduced by 43% |
| Managers with live visibility | 0 | 200+ |
| Report generation time | 3–4 hours | Instant |
| Manual inventory audits | Weekly | Automated, continuous |

---

## 🏗️ Architecture

```
┌────────────────────────────────────────────────────┐
│              Angular 12 Frontend                   │
│  ┌──────────┐  ┌──────────┐  ┌───────────────────┐│
│  │Dashboard │  │ Inventory│  │ Analytics / Charts ││
│  │ Overview │  │   Grid   │  │  (Chart.js + D3)   ││
│  └──────────┘  └──────────┘  └───────────────────┘│
└──────────────────────┬─────────────────────────────┘
                       │ REST API
             ┌─────────▼──────────┐
             │  Spring Boot API   │
             │  ┌──────────────┐  │
             │  │ Inventory Svc│  │
             │  │ Analytics Svc│  │
             │  │ Alert Engine │  │
             │  └──────────────┘  │
             └─────────┬──────────┘
                       │
             ┌─────────▼──────────┐
             │      IBM DB2        │
             │  (Stored Procs,     │
             │   Complex Queries,  │
             │   Aggregations)     │
             └────────────────────┘
```

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Angular 12, TypeScript, RxJS, Chart.js, Angular Material |
| State Management | NgRx (Redux pattern for Angular) |
| Backend | Spring Boot 3.x, Java 17, Spring Data JDBC |
| Database | IBM DB2, complex stored procedures, PL/SQL |
| Auth | JWT, Spring Security, RBAC |
| Export | Apache POI (Excel), iText (PDF) |
| Real-time | Server-Sent Events (SSE) for live stock updates |
| DevOps | Docker, Jenkins, Git |

---

## 📁 Project Structure

```
inventory-analytics-dashboard/
├── frontend/
│   ├── src/
│   │   ├── app/
│   │   │   ├── core/               # Auth, interceptors, guards
│   │   │   ├── shared/             # Reusable components, pipes
│   │   │   ├── dashboard/          # Overview widgets, KPI cards
│   │   │   ├── inventory/          # Inventory grid, search, filters
│   │   │   ├── analytics/          # Charts, trends, forecasts
│   │   │   ├── alerts/             # Low-stock alert management
│   │   │   └── store/              # NgRx state (actions, reducers)
│   │   ├── environments/
│   │   └── assets/
│   ├── angular.json
│   └── Dockerfile
├── backend/
│   ├── src/main/java/
│   │   ├── controller/
│   │   │   ├── InventoryController.java
│   │   │   ├── AnalyticsController.java
│   │   │   └── AlertController.java
│   │   ├── service/
│   │   ├── repository/
│   │   │   └── InventoryRepository.java   # DB2 stored proc calls
│   │   ├── model/
│   │   └── scheduler/              # Auto-alert scheduler
│   ├── src/main/resources/
│   │   └── db/
│   │       └── stored-procedures/  # DB2 SQL scripts
│   └── pom.xml
├── docker-compose.yml
└── README.md
```

---

## ⚙️ Setup & Run

### Prerequisites
- Node.js 16+
- Java 17+
- IBM DB2 (or use Docker image)
- Docker

### 1. Clone the repo

```bash
git clone https://github.com/MeenakshiGurrala/Projects.git
cd Projects/inventory-analytics-dashboard
```

### 2. Start DB2 and backend

```bash
# Start DB2 via Docker
docker run -e LICENSE=accept -e DB2INST1_PASSWORD=password \
  -p 50000:50000 ibmcom/db2

# Apply DB schema and stored procedures
cd backend/src/main/resources/db
db2 -tf stored-procedures/inventory_aggregations.sql

# Start Spring Boot
cd backend && ./mvnw spring-boot:run
```

### 3. Start Angular frontend

```bash
cd frontend
npm install
ng serve
# Access at http://localhost:4200
```

### 4. Or use Docker Compose

```bash
docker-compose up --build
```

---

## 🔌 Key API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/inventory` | All inventory with filters & pagination |
| `GET` | `/api/inventory/{sku}` | Single SKU detail + location breakdown |
| `GET` | `/api/inventory/low-stock` | Items below threshold |
| `GET` | `/api/analytics/trends` | Stock trend data (configurable window) |
| `GET` | `/api/analytics/turnover` | Inventory turnover rate by category |
| `POST` | `/api/inventory/adjust` | Manual stock adjustment with audit |
| `GET` | `/api/alerts` | Active low-stock alerts |
| `GET` | `/api/export/excel` | Export current inventory to Excel |

---

## 🔐 Role-Based Access

| Role | Access |
|---|---|
| `VIEWER` | Read-only dashboard and analytics |
| `MANAGER` | View + export + acknowledge alerts |
| `ADMIN` | Full access + user management + thresholds |
| `AUDITOR` | Historical data + audit trail access |

---

## 📊 Dashboard Features

- **KPI Cards** — Total SKUs, items in stock, low-stock count, value at risk
- **Live Inventory Grid** — Search, filter by warehouse/category, sortable columns
- **Trend Charts** — 7/30/90-day stock movement per SKU or category
- **Turnover Analysis** — Identify fast movers vs. slow movers
- **Alert Center** — Configurable thresholds, escalation rules, acknowledgement workflow
- **CSV / Excel Export** — One-click export of any filtered view

---

## 🧪 Testing

```bash
# Frontend unit tests
cd frontend && ng test

# Backend tests
cd backend && ./mvnw test

# E2E tests (Cypress)
cd frontend && npx cypress run
```

---

## 👩‍💻 Author

**Meenakshi Gurrala** — Java Full Stack Developer  
📧 gurralameenakshi99@gmail.com  
🔗 [LinkedIn](https://www.linkedin.com/in/meenakshi-gurrala-0bb6a5301/) · [GitHub](https://github.com/MeenakshiGurrala)
