# Learning & Training Platform – System Introduction

## Overview

This project is a full-stack learning and training platform designed to support online education, assessments, subscriptions and content delivery for students, managed through a centralized administrative web portal.

Students can access the platform using both a web application and an Android mobile application. The platform is designed as a scalable and modular system that supports real-world production use and future growth into a full training-institute and company-based learning platform.

The platform is built using:

- Web application for administrators and students
- Android mobile application for students
- Spring Boot backend APIs
- PostgreSQL as the primary relational database
- Redis for caching, session-related data and temporary tokens
- Object storage (S3 / MinIO) for images, documents and other media assets

The initial release supports only two roles:

- ADMIN
- STUDENT

Future versions will extend the platform to support training institutes, trainers, organizations and multi-tenant environments.

---

## Problem Statement

Most existing learning platforms focus either only on content delivery or only on assessments. They often lack:

- structured and enforceable access control based on subscription plans
- built-in reward and referral mechanisms for student engagement
- support for technical content using markdown and diagrams
- centralized administrative control and reporting
- a unified experience across both web and mobile platforms for students

This platform aims to combine:

- structured learning and technical content
- quizzes and assessments
- subscription-based feature and content access
- reward and referral driven engagement
- real-time notifications
- an extensible architecture for future analytics and AI-based features

---

## Primary Objectives

- Provide a unified learning platform accessible to students from both web and mobile applications
- Enable administrators to manage users, content, quizzes and subscriptions through a web portal
- Support paid subscriptions and billing workflows
- Provide engagement features such as daily rewards and referral programs
- Deliver a consistent API layer for both web and mobile clients
- Design the system to scale into a full training institute and organizational learning platform

---

## Target Users

### Administrator

- Manages users and roles
- Creates and publishes blogs and learning content
- Creates quizzes and assessments
- Manages subscription plans and access rules
- Sends announcements and system notifications
- Monitors user activity and platform reports

### Student

- Registers and logs in using either the web application or the Android mobile application
- Reads blogs and structured learning materials
- Takes quizzes and views results and progress
- Receives system and learning notifications
- Manages subscription status and personal profile
- Participates in reward and referral programs

---

## High Level System View

```mermaid
flowchart LR

StudentWeb[Student Web App]
StudentMobile[Student Android App]
AdminWeb[Admin Web App]

Gateway[Nginx / API Gateway]
Backend[Spring Boot Backend]

DB[(PostgreSQL)]
Cache[(Redis)]
Storage[(S3 / MinIO)]

StudentWeb --> Gateway
StudentMobile --> Gateway
AdminWeb --> Gateway

Gateway --> Backend

Backend --> DB
Backend --> Cache
Backend --> Storage
