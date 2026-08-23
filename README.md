# Hi, I'm Soumajit Sarkar 👋

## Senior Python Backend Engineer

I build backend systems, REST APIs, and third-party integrations using **Python, Django, Django REST Framework, and FastAPI**.

My focus is on reliable backend engineering involving **transactional workflows, asynchronous processing, concurrency control, database design, external integrations, and scalable API architectures**.

---

## 🚀 What I Build

* High-throughput REST APIs
* Python backend systems with Django, DRF, and FastAPI
* Transaction-heavy business workflows
* Third-party API integrations
* Asynchronous processing with RabbitMQ and Celery
* Redis-based caching and distributed locking
* Relational database systems
* Modular monolith architectures
* Authentication and role-based authorization
* Background jobs and scheduled processing
* Logging, auditing, and error-handling workflows

---

# ⭐ Featured Projects

## 1. Plex Gaming Wallet Gateway

### Production Backend Integration Platform

A scalable backend integration gateway for online gaming platforms, handling **game launches, player balance operations, wallet debits/credits, transaction recovery, and provider callbacks**.

The platform integrates multiple gaming providers and client platforms while maintaining wallet consistency under concurrent requests.

### System Scale

| Metric | Approximate Scale |
| :--- | ---: |
| API throughput | 1,000–1,500 RPS |
| Transactions | 1M+ / month |
| Provider integrations | 60+ |
| Client platforms | 70+ |
| Games | Thousands |

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
