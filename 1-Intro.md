# 🎓 Quiz & Subscription Learning Platform  
**Admin + Student | Web + Android | Spring Boot Backend | Object Storage (S3) | PostgreSQL | Redis**
** UI:   **
** backend: **
** cloud: **
** devops: cicd,jenkins   **

This project is a production-ready learning platform where:

- ✅ Admin manages quizzes, blogs and subscriptions  
- ✅ Students use **mobile app** and **web app**  
- ✅ Students can attempt quizzes, earn rewards, get notifications  
- ✅ Students can subscribe (Android → Google Play, Web → Razorpay)  
- ✅ Blogs are written in Markdown with **Mermaid diagram support**

---

## 🎯 Goal of this project

To build a single scalable platform that can later evolve into:

- a full training institute system
- and later a software company platform

For now, we keep it **simple and focused**:

👉 Only two roles  
- **ADMIN**
- **STUDENT**

---

## 🧱 High Level Architecture

```mermaid
flowchart LR

StudentMobile[Android App]
StudentWeb[Web App]
AdminWeb[Admin Web App]

StudentMobile --> API[Spring Boot API]
StudentWeb --> API
AdminWeb --> API

API --> DB[(PostgreSQL)]
API --> GooglePlay[Google Play Billing]
API --> Razorpay[Razorpay]
