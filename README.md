# Gaming Platform Infrastructure

## Overview

This repository contains all infrastructure resources required to deploy and run the Gaming Platform microservices.

It centralizes Docker Compose orchestration, Kubernetes manifests, infrastructure configuration, and deployment resources.

---

## Solution Architecture

The platform follows a Microservices Architecture combined with Event-Driven Architecture.

### Microservices

- UsersAPI
- CatalogAPI
- PaymentsAPI
- NotificationsAPI

### Infrastructure Components

- PostgreSQL
- RabbitMQ

---

## Architectural Principles

### Microservices

Each service has:

- Independent responsibility
- Independent deployment
- Independent scalability
- Isolated business logic

### Event-Driven Architecture

Services communicate asynchronously using RabbitMQ and MassTransit.

This approach minimizes coupling and improves system scalability.

---

## Registration Workflow

```text
UsersAPI
    │
    ▼
UserCreatedEvent
    │
    ▼
NotificationsAPI
```

### Description

1. User registers.
2. UsersAPI publishes UserCreatedEvent.
3. NotificationsAPI receives the event.
4. Welcome email is sent.

---

## Purchase Workflow

```text
CatalogAPI
    │
    ▼
OrderPlacedEvent
    │
    ▼
PaymentsAPI
    │
    ▼
PaymentProcessedEvent
    │
    ├────► CatalogAPI
    │
    └────► NotificationsAPI
```

### Description

1. User requests a purchase.
2. CatalogAPI publishes OrderPlacedEvent.
3. PaymentsAPI processes payment.
4. PaymentProcessedEvent is published.
5. CatalogAPI updates the user's library.
6. NotificationsAPI sends purchase confirmation.

---

## Technologies

- .NET 8
- ASP.NET Core
- PostgreSQL
- RabbitMQ
- MassTransit
- Docker Compose
- Kubernetes

---

## Docker Compose

Infrastructure services:

- PostgreSQL
- RabbitMQ

Application services:

- UsersAPI
- CatalogAPI
- PaymentsAPI
- NotificationsAPI

### Start Environment

```bash
docker compose up --build
```

### Stop Environment

```bash
docker compose down
```

---

## Kubernetes Resources

### Deployments

- users-api-deployment.yaml
- catalog-api-deployment.yaml
- payments-api-deployment.yaml
- notifications-api-deployment.yaml
- postgres-deployment.yaml
- rabbitmq-deployment.yaml

### Services

- users-api-service.yaml
- catalog-api-service.yaml
- payments-api-service.yaml
- notifications-api-service.yaml
- postgres-service.yaml
- rabbitmq-service.yaml

### Configuration

- configmap.yaml
- secret.yaml

### Namespace

- namespace.yaml

---

## ConfigMap

Stores non-sensitive configuration values:

- Environment names
- Internal service URLs
- RabbitMQ host
- Database host

---

## Secrets

Stores sensitive information:

- PostgreSQL credentials
- RabbitMQ credentials
- JWT signing key
- Connection strings

---

## Deployment Process

### Create Namespace

```bash
kubectl apply -f namespace.yaml
```

### Create ConfigMap

```bash
kubectl apply -f configmap.yaml
```

### Create Secret

```bash
kubectl apply -f secret.yaml
```

### Deploy Resources

```bash
kubectl apply -f .
```

### Verify Resources

```bash
kubectl get deployments
kubectl get pods
kubectl get services
```

---

## Project Goal

This project was developed as part of the FIAP Tech Challenge Phase 2, demonstrating:

- Microservices Architecture
- Event-Driven Communication
- Docker Compose Orchestration
- Kubernetes Deployments
- Configuration Management with ConfigMaps
- Secret Management with Kubernetes Secrets

---
