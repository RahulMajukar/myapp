
# Tech Stack – Learning & Training Platform

This document describes the complete and production-ready technology stack used
for the **Learning & Training Platform**.

The platform supports:

- Students using **Web App + Android Mobile App**
- Administrators using **Web App**
- A single scalable backend built using **Spring Boot**
- Subscription, rewards, referrals, blogs, quizzes and notifications

---

## Platform Overview

**Architecture Type**

- Modular Monolithic Backend (Spring Boot)
- Multi-client system (Web + Mobile)
- API-first design
- Cloud-ready and container friendly

---

## Client Applications

### 1. Student Web Application

**Recommended Stack**

- React (Vite or Next.js)
- Tailwind CSS
- Axios
- JWT based authentication
- WebSocket client for notifications

**Purpose**

- Student login & profile
- Read blogs and learning content (Markdown + Mermaid)
- Take quizzes and assessments
- View rewards, referrals and daily login points
- Subscription management
- Notifications

---

### 2. Student Android Application

**Stack**

- Kotlin
- Android Jetpack
- Retrofit
- Jetpack ViewModel + LiveData / StateFlow
- Firebase Cloud Messaging (optional for push)

**Purpose**

- Same student features as web
- Mobile first experience
- Offline caching (selected content)
- Push notifications

---

### 3. Admin Web Application

**Stack**

- React
- Tailwind CSS
- Rich Markdown Editor
- Mermaid diagram editor support
- File upload support

**Purpose**

- Manage users
- Manage roles
- Create blogs and learning content
- Create quizzes
- Manage subscriptions and plans
- Send notifications
- View reports and analytics

---

## Backend Technology

### Core Backend

**Stack**

- Java 17+
- Spring Boot 3+
- Spring Web
- Spring Security
- Spring Validation
- Spring Data JPA

**Purpose**

- Unified API for web and mobile clients
- Authentication and authorization
- Business logic
- Workflow orchestration
- Subscription enforcement
- Reward and referral processing

---

### Authentication & Security

**Stack**

- Spring Security
- JWT Access Token + Refresh Token
- BCrypt password hashing

**Features**

- Role based access (ADMIN, STUDENT)
- Device login tracking (web / mobile)
- Refresh token rotation
- Password reset tokens
- Account status handling

---

## Database Layer

### Primary Database

**Database**

- PostgreSQL

**Reason**

- Strong relational integrity
- Perfect for subscriptions, billing, rewards, referrals
- Transaction safety
- Reporting and analytics support

---

### ORM

- Spring Data JPA
- Hibernate

---

### Database Usage

Stores:

- users
- roles
- devices
- login history
- blogs
- markdown content
- quizzes
- quiz attempts
- subscriptions
- payments
- rewards
- referrals
- notifications
- audit logs

---

## Cache & Fast Data

### Redis

**Usage**

- OTP and password reset tokens
- login session metadata
- rate limiting
- daily login reward processing
- frequently accessed content cache
- temporary payment session storage

---

## Media & File Storage

### Object Storage

**Options**

- AWS S3
- MinIO (self hosted)

**Stores**

- blog images
- thumbnails
- attachments
- profile pictures
- certificate files

---

## Content System

### Blog & Learning Content

**Format**

- Markdown
- Mermaid diagrams embedded inside markdown

**Rendering**

- Server sends raw markdown
- Web and mobile clients render markdown and mermaid

---

## Quiz & Assessment Engine

**Stack**

- Spring Boot services
- PostgreSQL
- Redis for attempt throttling and session handling

**Features**

- quiz templates
- timed quizzes
- negative marking (optional)
- score calculation
- attempt history

---

## Subscription & Billing

### Payment Gateway

- Razorpay

### Backend Handling

- Payment initiation
- Webhook verification
- Subscription activation
- Renewal logic
- Plan feature mapping

---

## Rewards & Referral System

### Backend Stack

- Spring Scheduler
- PostgreSQL
- Redis

### Features

- daily login rewards
- referral codes
- referral tracking
- referral conversion rewards
- reward ledger per user

---

## Notification System

### Channels

- WebSocket for web
- Push notifications for mobile
- In-app notification store

### Stack

- Spring WebSocket
- Redis Pub/Sub (optional for scaling)

---

## Real-Time Communication

- Spring WebSocket
- STOMP protocol

Used for:

- notifications
- live status updates
- admin announcements

---

## Reporting & Analytics

### Stack

- PostgreSQL
- SQL based aggregation
- Spring services for reporting APIs

Used for:

- active users
- content usage
- quiz statistics
- revenue reports
- reward & referral reports

---

## API Design

- RESTful APIs
- Versioned endpoints
- DTO based request/response
- OpenAPI / Swagger

---

## DevOps & Infrastructure

### Containerization

- Docker
- Docker Compose

---

### CI / CD

- GitHub Actions

---

### Web Server

- Nginx
- Reverse proxy
- TLS termination
- API gateway

---

### Deployment Targets

- Cloud VM
- Kubernetes (future ready)

---

## Observability

### Logging

- SLF4J
- Logback
- JSON logs (optional)

---

### Monitoring

- Spring Actuator
- Prometheus (optional)
- Grafana (optional)

---

## Background Processing

- Spring Scheduler
- Async execution
- Transactional event listeners

Used for:

- daily reward distribution
- referral verification
- subscription expiry handling
- reminder notifications

---

## Messaging (Future Ready)

- Redis Pub/Sub
- Kafka (optional future scale)

---

## AI & Recommendation Layer (Planned)

- AI service integration layer
- Recommendation engine APIs
- Learning analytics APIs

Used for:

- personalized content suggestions
- quiz difficulty adaptation
- performance insights

---

## Summary of Core Stack

| Layer | Technology |
------|-----------
Frontend (Web) | React, Tailwind
Mobile App | Android (Kotlin)
Backend | Spring Boot
Security | Spring Security, JWT
Database | PostgreSQL
Cache | Redis
Object Storage | S3 / MinIO
Payments | Razorpay
Notifications | WebSocket + Push
API Gateway | Nginx
CI/CD | GitHub Actions
Containers | Docker

---

This tech stack is intentionally designed to:

- support both web and mobile clients
- scale without rewriting the backend
- allow future institute and multi-tenant expansion
- support AI and analytics features later
- remain simple to maintain in the early phase
