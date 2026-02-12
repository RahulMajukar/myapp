
# System Architecture

## Overview

This document describes the overall architecture of the Learning & Training Platform.
The system is designed to support both **students (web + Android mobile)** and **administrators (web)** through a single backend API layer.

The architecture follows a modular monolithic backend approach with clearly separated
domain modules, making it easy to evolve into microservices in future phases.

The system is optimized for:

- multi-client access (Web + Mobile)
- subscription based access control
- content delivery (Markdown + Mermaid)
- quizzes and assessments
- rewards, referrals and notifications
- payment and billing workflows
- scalable media handling

---

## High Level Architecture

```mermaid
flowchart LR

StudentWeb[Student Web App]
StudentMobile[Student Android App]
AdminWeb[Admin Web App]

Gateway[Nginx / API Gateway]
Backend[Spring Boot Application]

DB[(PostgreSQL)]
Cache[(Redis)]
Storage[(S3 / MinIO)]
Payment[Razorpay]
Ws[WebSocket Server]

StudentWeb --> Gateway
StudentMobile --> Gateway
AdminWeb --> Gateway

Gateway --> Backend

Backend --> DB
Backend --> Cache
Backend --> Storage
Backend --> Payment
Backend --> Ws


flowchart TB

Controller[API Controllers]
Service[Application Services]
Domain[Domain Logic]
Repository[Data Access Layer]
Infra[Infrastructure Layer]

Controller --> Service
Service --> Domain
Service --> Repository
Service --> Infra
Repository --> DB[(PostgreSQL)]
Infra --> Cache[(Redis)]
Infra --> Storage[(S3 / MinIO)]


flowchart LR

StudentWeb --> API
StudentMobile --> API
AdminWeb --> API

API[Spring Boot REST API]


flowchart TB

Auth[Auth & Security]
User[User & Role Management]
Content[Blog & Learning Content]
Quiz[Quiz & Assessment]
Subscription[Subscription & Plans]
PaymentModule[Payment & Billing]
Notification[Notification]
Reward[Rewards & Referrals]
Media[Media Management]
Reporting[Reporting & Analytics]

Auth --> User
User --> Content
Content --> Quiz
Subscription --> Content
PaymentModule --> Subscription
Notification --> User
Reward --> User
Media --> Content
Reporting --> DB[(PostgreSQL)]


sequenceDiagram

participant Client as Web / Mobile App
participant API as Auth API
participant DB as PostgreSQL
participant Cache as Redis

Client->>API: Login request
API->>DB: Validate credentials
DB-->>API: User + roles
API->>Cache: Store session / token metadata
API-->>Client: Access token + Refresh token


flowchart LR

User --> Subscription
Subscription --> Plan
Plan --> Feature

Feature --> APIAccess[API Access Rules]
Feature --> ContentAccess[Content & Quiz Access]


flowchart TB

Admin --> Editor[Markdown Editor]
Editor --> API
API --> DB[(Content Metadata)]
API --> Storage[(Images / Media)]
StudentWeb --> API
StudentMobile --> API


sequenceDiagram

participant Student
participant Client
participant QuizAPI
participant DB

Student->>Client: Start quiz
Client->>QuizAPI: Load quiz
QuizAPI->>DB: Load questions
DB-->>QuizAPI: Questions
QuizAPI-->>Client: Quiz data

Student->>Client: Submit answers
Client->>QuizAPI: Submit attempt
QuizAPI->>DB: Save answers
QuizAPI->>DB: Compute result
QuizAPI-->>Client: Score and result
