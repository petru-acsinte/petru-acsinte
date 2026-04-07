# Hi, I'm Petru 👋

Senior Java backend developer with 20+ years of experience building and optimizing enterprise systems, specializing in REST APIs, performance tuning, and large-scale data processing — including improvements that reduced API response times by up to 70%.

My GitHub projects focus on modernizing that foundation through a hands-on system design project exploring cloud-native architecture: microservices, containerization, CI/CD, and cloud deployment.

---

## 🚀 What I'm Building

### [Order Processing — Portfolio Project](https://github.com/petru-acsinte-dev)

A hands-on modernization project that deliberately follows the evolution of a real-world backend system.

The system simulates a distributed order processing platform with authentication, order management, and fulfillment services — with external REST APIs and internal asynchronous messaging via RabbitMQ.

| Phase | Repo | Description |
|---|---|---|
| Phase 1 | [order-processing-monolith](https://github.com/petru-acsinte-dev/order-processing-monolith) | Production-style Spring Boot monolith with JWT security, Flyway, Testcontainers, JaCoCo, Docker, CI/CD |
| Phase 2 | [order-processing-users](https://github.com/petru-acsinte-dev/order-processing-users) | Users & authentication microservice |
| Phase 2 | [order-processing-orders](https://github.com/petru-acsinte-dev/order-processing-orders) | Products & orders microservice |
| Phase 2 | [order-processing-shipments](https://github.com/petru-acsinte-dev/order-processing-shipments) | Fulfillment & shipment microservice |
| Phase 3 | AWS deployment | EC2, RDS Postgres, RabbitMQ, Nginx reverse proxy — live |
| Shared | [order-processing-common](https://github.com/petru-acsinte-dev/order-processing-common) | Shared library published to GitHub Packages |

🌐 **Live demo:** http://52.205.87.85/

### Try it live
1. Open the [Users API](http://52.205.87.85/users/swagger-ui/index.html) and POST to `/login/auth`
2. Login as `ADMIN/ADMIN` (full access) or `guest/guest` (read-only)
3. Copy the JWT, click **Authorize** in any service, paste the token
4. Explore the Orders and Shipments APIs

*Note* > ⚠️ Running on AWS free tier — first request may be slow due to JVM warmup on constrained memory.

---

## 🏗️ Architecture

```
                        ┌─────────────────────────────────────────────────────┐
                        │                   External Clients                  │
                        └─────────────────────┬───────────────────────────────┘
                                              │ REST (JWT)
                        ┌─────────────────────▼───────────────────────────────┐
                        │           Nginx Reverse Proxy (EC2, port 80)        │
                        └──────┬──────────────┬──────────────────┬────────────┘
                               │              │                  │
               ┌───────────────▼──┐  ┌────────▼───────┐  ┌──────▼──────────┐
               │   Users Service  │  │ Orders Service │  │     Shipments   │
               │   (port 8080)    │  │  (port 8081)   │  │     Service     │
               │                  │  │                │  │   (port 8082)   │
               │  - JWT issuance  │  │ - Products     │  │ - Fulfillments  │
               │  - Auth          │  │ - Orders       │  │ - Shipments     │
               └───────┬──────────┘  └───────┬────────┘  └──────┬──────────┘
                       │                     │                   │
               ┌───────▼──────┐              │                   │
               │   users_db   │      ┌───────▼──────┐   ┌───────▼──────┐
               │  (Postgres)  │      │   orders_db  │   │ shipments_db │
               └──────────────┘      │  (Postgres)  │   │  (Postgres)  │
                                     └──────────────┘   └──────────────┘
                                              │                   │
                                     ┌────────▼───────────────────▼────────┐
                                     │         RabbitMQ / Amazon MQ        │
                                     │                                      │
                                     │  Exchange: order.events (topic)      │
                                     │                                      │
                                     │  order.confirmed ──► shipment queue  │
                                     │  order.shipped   ──► orders queue    │
                                     │                                      │
                                     │  Dead-letter queues per queue        │
                                     └──────────────────────────────────────┘
```

**Flow:** Client authenticates via Users → receives JWT → calls Orders with JWT → order confirmed → `OrderConfirmedEvent` published to RabbitMQ → Shipments consumes → creates fulfillment and shipment → `OrderShippedEvent` published → Orders consumes → marks order as shipped.

---

**Key capabilities demonstrated:**

- Monolith → microservices decomposition
- JWT authentication with cross-service token validation (roles + externalId embedded in token — no inter-service user lookups)
- Independent CI/CD pipelines per service (GitHub Actions)
- Containerization with Docker, images published to GHCR
- Structured JSON logging with correlation ID (`X-Request-ID`) propagated across services and through message events
- Asynchronous messaging via RabbitMQ (topic exchange, DLQs, Saga pattern foundation)
- Database-per-service pattern (Postgres with schema isolation)
- Shared library pattern for cross-cutting concerns (security filters, exception handling, events, messaging config)
- Self-contained integration tests via Testcontainers (Postgres + RabbitMQ — no external dependencies)
- AWS deployment (EC2 + RDS + Nginx — live at http://52.205.87.85/)

---

## 🛠️ Skills & Technologies

**Experienced with:** Java · SQL · REST APIs · DB2 · Oracle · SQL Server · Eclipse · Maven

**Currently applying and expanding skills in:** Spring Boot · Spring Security · JWT ·
Microservices · Docker · GitHub Actions · Testcontainers · JaCoCo ·
PostgreSQL · Flyway · MapStruct · RabbitMQ · AWS

---

## 📋 Professional Background

20+ years delivering enterprise backend solutions:

- REST API design and implementation at scale
- Performance optimization — reduced API response times by up to 70%
- Data warehouse and ETL design across multiple DB platforms
- Automation tools that eliminated repetitive L2 support workflows
- Mentoring and cross-functional collaboration in regulated environments

---

## 📫 Connect

[LinkedIn](https://www.linkedin.com/in/petru-acsinte)

