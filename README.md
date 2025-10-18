# 🏠 AirBnB Clone — Backend API

[![Build Status](https://img.shields.io/badge/build-passing-brightgreen)]()
[![Coverage](https://img.shields.io/badge/coverage---blue)]()
[![License](https://img.shields.io/badge/license-MIT-lightgrey)]()

## 📖 Project Overview
This repository contains the backend service for an AirBnB-like platform. It provides a scalable RESTful + GraphQL API that powers user management, property listings, bookings, payments, and reviews.

The service is built for clarity and extensibility so it can be used as a foundation for a modern, production-ready full-stack application.

## 🎯 Goals & Core Features
- Secure user registration, authentication, and profile management
- Property listing CRUD (create, read, update, delete)
- Booking system with availability checks and booking lifecycle management
- Payment processing integration and transaction handling
- Reviews and ratings per property
- Performance optimizations: database indexing, caching (Redis)
- Background processing for async tasks (Celery)

## 🛠️ Technology Stack
- Backend framework: Django & Django REST Framework
- GraphQL: Graphene or equivalent (GraphQL API alongside REST)
- Database: PostgreSQL
- Async task queue: Celery
- Caching & message broker: Redis
- Containerization: Docker & docker-compose
- API docs: OpenAPI / Swagger

---

## 📚 Table of Contents
- [Quickstart (Docker)](#quickstart-docker)
- [Local Development](#local-development)
- [Environment Variables](#environment-variables)
- [Database & Migrations](#database--migrations)
- [Running Background Workers](#running-background-workers)
- [Testing](#testing)
- [API Documentation](#api-documentation)
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)

---

## 👥 Team Roles

This project benefits from a cross-functional team. Below are common roles (informed by the project overview and standard role descriptions such as those from ITRexGroup) and what each is responsible for in this repository:

- Backend Developer  
  Responsible for designing and implementing the server-side application: Django models, serializers, views, GraphQL schema, authentication, business logic, and unit/integration tests. Ensures APIs are versioned, documented, and performant.

- Database Administrator (DBA)  
  Designs and maintains the PostgreSQL schema, indexes, migrations, backup/restore strategies, and performance tuning. Works closely with backend developers to model booking and availability logic and to optimize queries for scale.

- DevOps / Infrastructure Engineer  
  Builds and maintains containerization (Docker), docker-compose or Kubernetes manifests, CI/CD pipelines, deployment automation, and environment provisioning. Manages infrastructure-as-code, monitoring, logging, and application telemetry.

- Site Reliability / Operations Engineer (SRE)  
  Ensures reliability and uptime: monitoring, alerting, autoscaling, incident response, runbooks, and SLAs. Works with DevOps to improve fault-tolerance and to perform capacity planning.

- QA Engineer / Test Engineer  
  Creates and runs test plans, automated test suites (unit, integration, end-to-end), and regression tests for booking/payment flows and security-sensitive endpoints. Tracks bugs and verifies fixes before releases.

- Frontend Developer (Integration)  
  Implements the client-side integration with the backend APIs (authentication flows, listing and booking UI, GraphQL/REST queries). Collaborates on API design, CORS, and pagination/serialization expectations.

- UX / UI Designer  
  Designs user flows, wireframes, and interface patterns for property search, booking, and account management to ensure usability and accessibility. Provides assets and interaction specs for frontend implementation.

- Product Manager / Owner  
  Prioritizes features and defines acceptance criteria, coordinates stakeholders, writes user stories, and helps guide the roadmap and releases. Ensures the team focuses on customer value and compliance requirements.

- Security Engineer  
  Conducts threat modeling, security reviews, and vulnerability assessments. Responsible for secure configuration, secrets management, authentication/authorization design, and guidance for data protection and PCI considerations for payments.

- Data Engineer / Analytics  
  Designs event tracking, ETL pipelines, reporting schemas and supports analytics needs (e.g., occupancy rates, revenue reports). Ensures data quality and provides datasets for product and business decisions.

These roles can overlap depending on team size; in smaller teams, individuals may wear multiple hats. Responsibilities should be adapted to match the team's composition and the project phase.

---

## 🚀 Quickstart (Docker)
Recommended for getting the full stack (DB, Redis, app, Celery) running quickly.

1. Copy or create the .env file (see [Environment Variables](#environment-variables)).
2. Build and start containers:
   ```bash
   docker-compose up --build
   ```
3. Apply DB migrations:
   ```bash
   docker-compose exec web python manage.py migrate
   ```
4. Create a superuser (optional):
   ```bash
   docker-compose exec web python manage.py createsuperuser
   ```
5. The API will be available at http://localhost:8000 (or the port defined in docker-compose).

---

## 🧰 Local Development (without Docker)
1. Create and activate a Python virtual environment:
   ```bash
   python -m venv .venv
   source .venv/bin/activate   # macOS / Linux
   .venv\Scripts\activate      # Windows
   ```
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Configure your environment (see [Environment Variables](#environment-variables)).
4. Run migrations and start the dev server:
   ```bash
   python manage.py migrate
   python manage.py runserver
   ```

---

## 🔑 Environment Variables
Create a `.env` file at the repo root (example values shown):

```env
# Django
DJANGO_SECRET_KEY=your_secret_key_here
DJANGO_DEBUG=True

# Database (Postgres)
POSTGRES_DB=airbnb
POSTGRES_USER=airbnb_user
POSTGRES_PASSWORD=securepassword
POSTGRES_HOST=db
POSTGRES_PORT=5432

# Redis
REDIS_URL=redis://redis:6379/0

# Celery
CELERY_BROKER_URL=${REDIS_URL}
CELERY_RESULT_BACKEND=${REDIS_URL}

# Other
ALLOWED_HOSTS=localhost,127.0.0.1
```

Adjust values for production use and keep secrets out of version control.

---

## 🗃️ Database & Migrations
- To create and apply migrations:
  ```bash
  python manage.py makemigrations
  python manage.py migrate
  ```
- Use Django fixtures or custom scripts to seed sample data for development.

---

## 🔄 Running Background Workers (Celery)
Start the worker (Docker):
```bash
docker-compose exec worker celery -A project_name worker --loglevel=info
```
Start a beat scheduler for periodic tasks:
```bash
docker-compose exec worker celery -A project_name beat --loglevel=info
```
Replace project_name with your Django project module name.

---

## ✅ Testing
Run tests locally:
```bash
python manage.py test
```
Or inside Docker:
```bash
docker-compose exec web python manage.py test
```
Aim to include unit tests for models, serializers, views, and integration tests for booking/payment flows.

---

## 📜 API Documentation
- REST: OpenAPI / Swagger endpoint (e.g., `/api/docs/` or `/swagger/`) — add or update drf-yasg / drf-spectacular config as needed.
- GraphQL: GraphiQL or GraphQL Playground at `/graphql/` for exploring queries & mutations.

Include examples for authentication, booking creation, and payment flows in your docs.

---

## ♻️ Caching & Performance
- Cache read-heavy endpoints (listings, property details) using Redis.
- Add database indexes for frequently filtered fields (e.g., property.location, booking dates).
- Use select_related / prefetch_related to reduce N+1 queries.

---

## 🤝 Contributing
Contributions are welcome. A basic workflow:
1. Fork the repo
2. Create a feature branch: git checkout -b feat/your-feature
3. Run tests and linters locally
4. Open a pull request describing your changes

Please follow the code style (PEP8), include tests, and keep PRs focused.

---

## 🧾 License
This project is licensed under the MIT License. See LICENSE for details.

---

## 📬 Contact
Maintainer: Einsteinke-hub
For questions, feature requests, or issues — open an issue in the repo or reach out via your preferred channel.

```
