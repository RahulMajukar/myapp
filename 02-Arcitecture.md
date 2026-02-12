---
title: Learning & Training Platform - System Architecture
---
# System Architecture

## Overview

This document describes the overall architecture of the Learning & Training Platform.
The system supports both **students (web and Android mobile applications)** and
**administrators (web application)** through a single backend API layer.

The backend follows a modular monolithic architecture with clearly separated domain
modules. This design allows the system to evolve into microservices in future phases
without major restructuring.

The platform is optimized for:

- multi-client access (web and mobile)
- subscription based access control
- markdown based content delivery with Mermaid diagrams
- quizzes and assessments
- rewards, referrals and notifications
- payment and billing workflows
- scalable media storage

---

## High-Level Architecture

flowchart LR
    subgraph Clients
        A[Student Web App]
        B[Student Android App]
        C[Admin Web App]
    end
    
    subgraph Gateway
        D[Nginx / API Gateway]
    end
    
    subgraph Backend
        E[Spring Boot Application]
    end
    
    subgraph Storage
        F[(PostgreSQL)]
        G[(Redis)]
        H[(S3 / MinIO)]
    end
    
    subgraph External
        I[Razorpay]
        J[WebSocket / Notification Channel]
    end
    
    A & B & C --> D
    D --> E
    E --> F & G & H
    E --> I
    E --> J

---

## Application Layer Architecture

flowchart TB
    subgraph Presentation
        A[API Controllers]
    end
    
    subgraph Application
        B[Application Services]
    end
    
    subgraph Domain
        C[Domain Layer]
    end
    
    subgraph Persistence
        D[Repository Layer]
    end
    
    subgraph Infrastructure
        E[Infrastructure Layer]
    end
    
    subgraph Data
        F[(PostgreSQL)]
        G[(Redis)]
        H[(S3 / MinIO)]
    end
    
    A --> B
    B --> C
    B --> D
    B --> E
    D --> F
    E --> G
    E --> H

---

## Client Communication Flow

flowchart LR
    A[Student Web App] --> D[Spring Boot REST API]
    B[Student Android App] --> D
    C[Admin Web App] --> D

---

## Domain Module Architecture

flowchart TB
    subgraph Security
        Auth[Authentication & Security]
    end
    
    subgraph Core
        User[User & Role Management]
    end
    
    subgraph Content
        Content[Blog & Learning Content]
        Quiz[Quiz & Assessment]
    end
    
    subgraph Commerce
        Subscription[Plans & Subscriptions]
        PaymentModule[Payment & Billing]
    end
    
    subgraph Engagement
        Notification[Notification Service]
        Reward[Rewards & Referrals]
    end
    
    subgraph Support
        Media[Media Management]
        Reporting[Reporting & Analytics]
    end
    
    subgraph Database
        DB[(PostgreSQL)]
    end
    
    Auth --> User
    User --> Content
    Content --> Quiz
    Subscription --> Content
    PaymentModule --> Subscription
    Notification --> User
    Reward --> User
    Media --> Content
    Reporting --> DB

---

## Authentication Flow

sequenceDiagram
    participant Client as Web / Mobile App
    participant API as Auth API
    participant DB as PostgreSQL
    participant Cache as Redis
    
    Client->>API: POST /auth/login
    API->>DB: SELECT users WHERE email = ?
    DB-->>API: User + roles
    API->>API: Validate password
    API->>Cache: SET refresh_token:{id}
    API-->>Client: 200 OK (access + refresh tokens)

---

## Subscription & Access Control

flowchart LR
    User -->|Has| Subscription
    Subscription -->|Belongs to| Plan
    Plan -->|Contains| Feature
    
    Feature -->|Grants| APIAccess[API Access Rules]
    Feature -->|Grants| ContentAccess[Content & Quiz Access]

---

## Content Management Flow

flowchart TB
    subgraph Admin
        Admin[Administrator]
        Editor[Markdown Editor]
    end
    
    subgraph Backend
        API[Content API]
    end
    
    subgraph Storage
        DB[(Content Metadata)]
        Media[(Images & Media)]
    end
    
    subgraph Clients
        StudentWeb[Student Web App]
        StudentMobile[Student Android App]
    end
    
    Admin --> Editor
    Editor --> API
    API --> DB
    API --> Media
    StudentWeb --> API
    StudentMobile --> API
    DB --> StudentWeb
    DB --> StudentMobile

---

## Quiz Assessment Flow

sequenceDiagram
    participant Student
    participant Client as Web/Mobile App
    participant QuizAPI as Quiz Service
    participant DB as Database
    
    Student->>Client: Start quiz
    Client->>QuizAPI: GET /quizzes/{id}
    QuizAPI->>DB: SELECT questions WHERE quiz_id = ?
    DB-->>QuizAPI: Questions
    QuizAPI-->>Client: Quiz data
    
    Student->>Client: Submit answers
    Client->>QuizAPI: POST /attempts
    QuizAPI->>DB: INSERT attempt answers
    QuizAPI->>DB: Calculate score
    QuizAPI-->>Client: Result and score

---

## Notification System

flowchart TB
    subgraph Events
        Event[System Event]
    end
    
    subgraph Processing
        NotificationService[Notification Service]
    end
    
    subgraph Storage
        DB[(PostgreSQL)]
    end
    
    subgraph Delivery
        Ws[WebSocket Channel]
        Email[Email Service]
        Push[Push Notifications]
    end
    
    subgraph Clients
        Client[Web / Mobile App]
    end
    
    Event --> NotificationService
    NotificationService --> DB
    NotificationService --> Ws
    NotificationService --> Email
    NotificationService --> Push
    Ws --> Client
    Email --> Client
    Push --> Client

---

## Media Upload Flow

flowchart LR
    subgraph Admin
        Admin[Administrator]
        UploadAPI[Upload API]
    end
    
    subgraph Storage
        Storage[(S3 / MinIO)]
        DB[(media_files)]
    end
    
    Admin --> UploadAPI
    UploadAPI --> Storage
    UploadAPI --> DB

---

## Payment Flow

sequenceDiagram
    participant Student
    participant Client as Web/Mobile App
    participant API as Payment API
    participant Razorpay as Razorpay
    participant DB as Database
    
    Student->>Client: Select subscription plan
    Client->>API: POST /payments/create-order
    API->>Razorpay: Create order
    Razorpay-->>API: Order details
    API-->>Client: Payment UI session
    
    Client->>Razorpay: Process payment
    Razorpay->>API: Webhook callback
    API->>DB: Update payment status
    API->>DB: Activate subscription
    API-->>Student: Payment confirmation

---

## Scheduled Jobs

flowchart TB
    subgraph Scheduler
        Scheduler[Spring Scheduler / Quartz]
    end
    
    subgraph Services
        RewardEngine[Reward Engine]
        NotificationService[Notification Service]
    end
    
    subgraph Database
        DB[(PostgreSQL)]
    end
    
    Scheduler --> RewardEngine
    Scheduler --> NotificationService
    RewardEngine --> DB
    NotificationService --> DB

---

## Data Flow Diagram (DFD – Level 1)

flowchart LR
    subgraph Actors
        Student[Student (Web / Mobile)]
        Admin[Administrator]
    end
    
    subgraph System
        API[Spring Boot Backend]
    end
    
    subgraph Data
        DB[(PostgreSQL)]
        Cache[(Redis)]
        Storage[(S3 / MinIO)]
    end
    
    subgraph External
        Payment[Razorpay]
    end
    
    Student -->|Login / Content / Quiz / Subscription| API
    Admin -->|Manage content / users / plans| API
    
    API -->|Read / Write| DB
    API -->|Tokens / Cache / OTP| Cache
    API -->|Upload / Fetch media| Storage
    API -->|Create order / Verify payment| Payment
    
    Payment -->|Webhook events| API
    API -->|Responses & notifications| Student
    API -->|Admin responses| Admin

---

## Deployment View

flowchart LR
    subgraph Internet
        User[User Requests]
    end
    
    subgraph LoadBalancer
        Nginx[Nginx]
    end
    
    subgraph Application
        Backend[Spring Boot Application]
    end
    
    subgraph DataLayer
        DB[(PostgreSQL)]
        Cache[(Redis)]
        Storage[(S3 / MinIO)]
    end
    
    User --> Nginx
    Nginx --> Backend
    Backend --> DB
    Backend --> Cache
    Backend --> Storage

---

## Technology Stack Summary

| Layer | Technology |
|-------|------------|
| **Frontend Web** | React.js |
| **Mobile** | Android Native (Kotlin/Java) |
| **Backend** | Java 17+, Spring Boot 3.x |
| **Database** | PostgreSQL 15+ |
| **Cache** | Redis 7+ |
| **Storage** | S3/MinIO |
| **API Gateway** | Nginx |
| **Payment** | Razorpay |
| **Authentication** | JWT + Refresh Tokens |
| **Real-time** | WebSocket (STOMP) |
| **Scheduling** | Spring Scheduler/Quartz |
| **Container** | Docker |
| **Orchestration** | Kubernetes (future) |
