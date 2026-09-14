# Tech Challenge - Fase 3

## Overview

This project implements a distributed microservices architecture for the FIAP Cloud Games platform.

The solution was enhanced during Phase 3 to address scalability, observability, performance, security, and infrastructure optimization requirements through the adoption of modern cloud-native technologies.

---

# Architecture

```text
                     Internet
                         │
                         ▼
                 Kong API Gateway
                         │
 ┌───────────────┬───────────────┬───────────────┐
 │               │               │               │
 ▼               ▼               ▼               ▼
UsersAPI     CatalogAPI     PaymentsAPI    Notifications
                                             Lambda
                                                    ▲
                                                    │
                                            Amazon SQS
```

Supporting Services:

```text
PostgreSQL
Redis
MongoDB
RabbitMQ
Prometheus
Grafana
AWS Lambda
Amazon SQS
```

---

# Microservices

## UsersAPI

Responsible for:

- User registration
- Authentication
- JWT generation
- User management

Technology:

- ASP.NET Core
- PostgreSQL
- RabbitMQ

---

## CatalogAPI

Responsible for:

- Game catalog management
- User library management
- Reviews management

Technology:

- ASP.NET Core
- PostgreSQL
- Redis
- MongoDB
- RabbitMQ

---

## PaymentsAPI

Responsible for:

- Payment processing
- Event publishing

Technology:

- ASP.NET Core
- RabbitMQ

---

## Notifications Lambda

Responsible for:

- Welcome notifications
- Purchase confirmation notifications
- Payment rejection notifications

Technology:

- AWS Lambda
- Amazon SQS

Repository:

```text
notifications-lambda
```

---

# API Gateway

## Kong

A Kong API Gateway was introduced as the single entry point of the platform.

Responsibilities:

- Request routing
- Traffic management
- Centralized access point

Example routes:

```text
/users
/catalog
/payments
```

---

# Observability

## Prometheus

Prometheus is responsible for collecting application metrics.

Collected metrics include:

- Total requests
- Request duration
- Active requests
- API performance indicators

---

## Grafana

Grafana dashboards provide real-time visibility into application behavior.

Monitored indicators:

- Request count
- Request latency
- Active requests
- Service health

---

# Distributed Cache

## Redis

Redis was introduced to improve application performance and reduce database load.

Implementation:

```text
CatalogAPI
    ↓
Redis Cache
    ↓
PostgreSQL
```

Cached resource:

```text
GET /games
```

Benefits:

- Reduced latency
- Reduced database queries
- Faster responses

---

# Polyglot Persistence

## MongoDB

MongoDB was introduced for storing game reviews.

Collection:

```text
reviews
```

Example document:

```json
{
  "gameId": "11111111-1111-1111-1111-111111111111",
  "userId": "22222222-2222-2222-2222-222222222222",
  "rating": 5,
  "comment": "Excellent game"
}
```

Benefits:

- Flexible schema
- NoSQL persistence
- Better support for document-oriented data

---

# Messaging

## RabbitMQ

RabbitMQ remains responsible for communication between core microservices.

Published events include:

- UserCreatedEvent
- PaymentProcessedEvent

---

# Serverless Architecture

## Amazon SQS

Queue:

```text
notifications-queue
```

Purpose:

- Decouple services
- Process notifications asynchronously

---

## AWS Lambda

Function:

```text
notifications-lambda
```

Purpose:

- Process notification events
- Replace the previous NotificationsAPI container
- Reduce infrastructure costs

Architecture:

```text
Amazon SQS
        ↓
notifications-queue
        ↓
notifications-lambda
        ↓
Notification Processing
```

Benefits:

- Event-driven processing
- Automatic scaling
- Reduced resource consumption
- No continuously running container

---

# Technology Stack

## Backend

- .NET 8
- ASP.NET Core

## Databases

- PostgreSQL
- MongoDB

## Cache

- Redis

## Messaging

- RabbitMQ
- Amazon SQS

## Observability

- Prometheus
- Grafana

## Cloud

- AWS Lambda

## API Gateway

- Kong

---

# Repositories

## Application

```text
game-store
```

Contains:

- UsersAPI
- CatalogAPI
- PaymentsAPI
- Shared

---

## Infrastructure

```text
game-store-infra
```

Contains:

- Docker Compose
- Kong Configuration
- Prometheus Configuration
- Grafana Configuration

---

## Serverless

```text
notifications-lambda
```

Contains:

- AWS Lambda implementation
- Notification handlers
- Event models

---

# Phase 3 Requirements Coverage

| Requirement | Status |
|------------|---------|
| API Gateway | ✅ |
| Serverless Architecture | ✅ |
| Observability | ✅ |
| MongoDB / NoSQL | ✅ |
| Redis Cache | ✅ |
| Distributed Messaging | ✅ |

---

# Team

FIAP Tech Challenge - Phase 3

Microservices Modernization and Cloud Native Architecture
