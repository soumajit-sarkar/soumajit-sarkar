# ⭐ Featured Projects

## 1. Plex Gaming Wallet Gateway

### Production Backend Integration Platform

A scalable backend integration gateway for online gaming platforms, responsible for game launches, player balance operations, wallet debits/credits, transaction recovery, and provider callbacks.

The platform integrates multiple gaming providers and client platforms while protecting wallet consistency under concurrent requests.

### System Scale

| Metric                | Approximate Scale |
| --------------------- | ----------------: |
| API throughput        |   1,000–1,500 RPS |
| Transactions          |       1M+ / month |
| Provider integrations |               60+ |
| Client platforms      |               70+ |
| Games                 |         Thousands |

### Architecture

```text
                    Client / Game Platform
                             │
                             ▼
                    ┌─────────────────┐
                    │    Core API     │
                    │     FastAPI     │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │    RabbitMQ     │
                    └────────┬────────┘
                             │
          ┌──────────────────┼──────────────────┐
          ▼                  ▼                  ▼
    Auth Consumer      Game Consumer      Wallet Consumer
          │                  │                  │
          └──────────────────┼──────────────────┘
                             │
                    ┌────────▼────────┐
                    │      Redis      │
                    │ Distributed     │
                    │     Locks       │
                    └────────┬────────┘
                             │
                    ┌────────▼────────┐
                    │      MySQL      │
                    │ Transactional   │
                    │     Data        │
                    └─────────────────┘

                         MongoDB
                            │
                            ▼
                     Application Logs
```

### Engineering Challenges

#### Concurrent Wallet Operations

Multiple requests can attempt to modify the same player's wallet concurrently.

Redis distributed locking is used to coordinate critical wallet operations and reduce the risk of conflicting updates.

#### Transaction Reliability

Wallet operations require reliable transaction tracking and recovery mechanisms so that interrupted or failed operations can be detected and reconciled.

#### Asynchronous Processing

RabbitMQ separates API-facing operations from asynchronous processing and provider-specific workloads.

#### Provider Integration Architecture

Different gaming providers expose different APIs and request/response formats.

Provider-specific integration modules isolate those differences from the core business workflows.

#### High-Throughput Processing

The platform operates at approximately 1,000–1,500 RPS, requiring careful handling of database operations, asynchronous workloads, concurrency, and external integrations.

### My Engineering Focus

* Backend API development
* Gaming provider integrations
* Wallet transaction workflows
* Concurrent transaction handling
* Redis distributed locking
* RabbitMQ-based asynchronous processing
* MySQL database operations
* MongoDB application logging
* External API communication using HTTPX
* Transaction recovery and reliability mechanisms

### Technology

`Python` `FastAPI` `RabbitMQ` `Redis` `MySQL` `SQLAlchemy` `MongoDB` `HTTPX`

---

# 2. Enterprise Order Management System (OMS)

### Modular Enterprise Order Management Platform

An enterprise Order Management System designed to manage the complete order lifecycle across products, inventory, payments, warehouses, invoices, refunds, notifications, reporting, and auditing.

The system is designed as a **modular monolith**, with business domains separated into dedicated Django applications while maintaining a single deployable backend.

### Architecture

```text
                         API Clients
                             │
                             ▼
                    ┌─────────────────┐
                    │   Django / DRF  │
                    │    REST API     │
                    └────────┬────────┘
                             │
          ┌──────────────────┼──────────────────┐
          │                  │                  │
          ▼                  ▼                  ▼
      Accounts           Products            Orders
          │                  │                  │
          │                  │                  ▼
          │                  │             Inventory
          │                  │                  │
          │                  │                  ▼
          │                  │              Warehouse
          │                  │
          │                  ▼
          │              Payments
          │                  │
          └──────────────────┼──────────────────┐
                             │                  │
                             ▼                  ▼
                         Invoices             Refunds
                             │                  │
                             └────────┬─────────┘
                                      │
                                      ▼
                                MySQL Database

                ┌──────────────────────────────┐
                │            Redis             │
                │ Cache / Locks / Retry State  │
                └──────────────────────────────┘

                ┌──────────────────────────────┐
                │           Celery             │
                │    Background Processing     │
                └──────────────────────────────┘

                ┌──────────────────────────────┐
                │          MongoDB              │
                │ Audit / Application / Error  │
                │ Access Logs                  │
                └──────────────────────────────┘
```

### Core Modules

* Authentication & Role-Based Access Control
* Department Management
* Product Management
* Inventory Management
* Order Management
* Payment Management
* Warehouse Management
* Invoice Management
* Refund Management
* Notification Management
* Reporting
* Audit Logging

### Engineering Focus

#### Modular Monolith Architecture

Business domains are isolated into dedicated Django applications with clear boundaries between responsibilities.

The architecture provides modularity without introducing unnecessary microservice complexity.

#### Order & Inventory Consistency

Order placement requires coordinated changes across order and inventory state.

Database transactions and appropriate locking strategies are used to maintain consistency during critical operations.

#### Concurrency Control

Inventory operations can be affected by concurrent order requests.

Redis-based locking and database-level transaction management are used where appropriate to protect critical workflows.

#### Asynchronous Processing

Celery handles background workloads that should not block API requests, including notifications, report generation, invoice-related processing, and retryable tasks.

#### Logging & Auditing

MongoDB is used for application-oriented logging, including:

* Audit logs
* Application logs
* Error logs
* Access logs

Retention and cleanup mechanisms are considered to prevent uncontrolled log growth.

### Technology

`Python` `Django` `Django REST Framework` `MySQL` `Redis` `Celery` `MongoDB` `Docker` `Swagger / OpenAPI`

---

# 3. Enterprise Management System (EMS)

### FastAPI + PostgreSQL Backend System

An enterprise management backend built with FastAPI and PostgreSQL, designed around structured REST APIs, relational data modeling, validation, business logic separation, and production-oriented backend practices.

The project focuses on maintainable backend architecture rather than simply exposing database CRUD operations.

### Architecture

```text
                         API Clients
                             │
                             ▼
                    ┌─────────────────┐
                    │     FastAPI     │
                    │     REST API    │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │   API / Router  │
                    │     Layer       │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │ Business Logic  │
                    │ / Service Layer │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │    SQLAlchemy   │
                    │       ORM       │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │   PostgreSQL    │
                    └─────────────────┘
```

### Engineering Focus

* FastAPI REST API development
* PostgreSQL database design
* SQLAlchemy-based persistence
* Pydantic request/response validation
* Separation of API and business logic
* Dependency injection
* Centralized error handling
* Environment-based configuration
* OpenAPI API documentation
* Automated testing with pytest
* Production deployment preparation

### Technology

`Python` `FastAPI` `PostgreSQL` `SQLAlchemy` `Pydantic` `pytest` `OpenAPI`

---

# 🧠 Backend Engineering Focus

I particularly enjoy working on problems involving:

### API Design

Designing REST APIs with clear resource boundaries, validation, consistent error handling, pagination, filtering, and API documentation.

### Database Design

Designing relational schemas, relationships, constraints, indexes, transactions, and query patterns for business-critical applications.

### Concurrency

Handling race conditions in transaction-heavy systems using database transactions, locking strategies, and Redis-based distributed locks.

### Asynchronous Processing

Designing background workflows using RabbitMQ and Celery where synchronous request processing would create unnecessary latency or coupling.

### Integrations

Building reliable integrations with external APIs and adapting provider-specific protocols into consistent internal business workflows.

### Reliability

Thinking about retries, idempotency, transaction recovery, failure handling, logging, and consistency rather than treating the happy path as the entire system.

---

# 🛠️ Technical Stack

### Languages

`Python` `SQL`

### Backend

`Django` `Django REST Framework` `FastAPI` `SQLAlchemy`

### Databases

`MySQL` `PostgreSQL` `MongoDB`

### Distributed & Async

`Redis` `RabbitMQ` `Celery`

### API & Integration

`REST APIs` `HTTPX` `OpenAPI` `Swagger`

### Development & Deployment

`Git` `GitHub` `Docker` `Linux`

---

# 📌 Engineering Principles

I prefer:

* Clear separation of concerns
* Modular architecture
* Explicit business logic
* Database integrity over application assumptions
* Idempotent and retry-safe workflows
* Proper transaction boundaries
* Defensive error handling
* Observable systems with meaningful logs
* Simple architectures before unnecessary microservices
* Code that can be maintained by another engineer

---

# 📫 Let's Connect

I'm interested in working on backend engineering projects involving:

* Python backend development
* Django / DRF applications
* FastAPI services
* REST API development
* Third-party API integrations
* Transaction-heavy systems
* Backend architecture and modernization

**Open to full-time opportunities and international freelance backend projects.**

---

### GitHub

[github.com/soumajit-sarkar](https://github.com/soumajit-sarkar)
