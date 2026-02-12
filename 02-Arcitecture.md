
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
