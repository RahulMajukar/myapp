
# Application Modules

This platform is an e-learning and content platform for students and administrators.
Currently only Admin and Student roles are supported.

## 1. User & Role Management Module
Responsible for:
- User registration
- Login
- Role assignment
- Account status management

Roles:
- ADMIN
- STUDENT

Main features:
- profile management
- login history
- device tracking

---

## 2. Authentication & Session Module
Handles:
- JWT access token
- refresh token
- forgot password
- password reset
- logout
- token revocation

---

## 3. Course & Blog Module
Responsible for:
- markdown based blogs
- course pages
- content publishing
- mermaid diagram rendering

Admin creates content.
Students consume content.

---

## 4. Quiz & Assessment Module
Responsible for:
- quizzes
- questions
- options
- quiz attempts
- score calculation
- result history

---

## 5. Subscription & Access Module
Responsible for:
- plan management
- user subscription mapping
- content access control

---

## 6. Notification Module
Responsible for:
- system notifications
- subscription reminders
- quiz related alerts
- admin announcements

---

## 7. Rewards & Referral Module
Responsible for:
- daily login rewards
- referral rewards
- referral tracking
- reward history

---

## 8. Media Management Module
Responsible for:
- image uploads
- storage in S3 / MinIO
- returning public URLs
- media metadata tracking
