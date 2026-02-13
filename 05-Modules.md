System Modules – Learning & Training Platform
1. Purpose
This document describes all functional modules of the Learning & Training Platform (Admin + Student | Web + Android | Spring Boot Backend).

Each module is designed as an independent business domain inside a single Spring Boot application (modular monolith) so that it can be easily extracted as a microservice in future.

The platform currently supports only two roles:

ADMIN

STUDENT

Students can use both:

Web application

Android mobile application

2. Module Architecture Overview
flowchart TB
    subgraph Clients
        A[Student Web App]
        B[Student Android App]
        C[Admin Web App]
    end

    subgraph Backend Modules
        Auth[Authentication & Security]
        User[User & Role Management]
        Content[Blog & Learning Content]
        Quiz[Quiz & Assessment]
        Subscription[Subscription & Plans]
        Billing[Payment & Billing]
        Reward[Rewards & Referrals]
        Notification[Notification System]
        Media[Media Management]
        Activity[Activity & Audit]
        Reporting[Reporting & Analytics]
    end

    A --> Auth
    B --> Auth
    C --> Auth

    Auth --> User

    User --> Content
    User --> Quiz
    User --> Subscription
    User --> Reward
    User --> Notification

    Content --> Media
    Quiz --> Media

    Subscription --> Billing

    Activity --> Reporting

3. Authentication & Security Module
Purpose
Handles:

login and registration

JWT based authentication

refresh tokens

password reset

OTP and verification

device and platform tracking (web / mobile)

Why this module is needed
To securely control access for both web and mobile users and enforce role based authorization.

Main responsibilities
user login (email / mobile + password)

token issuing and refreshing

forgot password & reset flow

session tracking

blocking and disabling accounts

rate limiting support using Redis

Key features
JWT access token

refresh token stored in database

Redis for token metadata and OTP

platform info (WEB / ANDROID)

4. User & Role Management Module
Purpose
Manages platform users and their assigned roles.

Why this module is needed
To separate administration users and student users and allow future expansion to trainer and institute users.

Main responsibilities
create and update users

assign roles

activate / deactivate users

store profile information

manage login history

5. Content & Blog Module (Markdown + Mermaid)
Purpose
Provides technical blogs and learning content written in Markdown with Mermaid diagram support.

Why this module is needed
This platform is designed for technical training and documentation. Markdown + Mermaid allows:

code friendly content

architectural and flow diagrams inside articles

versioned learning material

Main responsibilities
create and edit posts

versioning of markdown content

preview and publish workflow

tagging and categorization

visibility based on subscription

Supported content
Markdown text

embedded Mermaid diagrams

image references

6. Quiz & Assessment Module
Purpose
Handles quizzes, assessments and student attempts.

Why this module is needed
To measure student learning and provide progress tracking.

Main responsibilities
quiz creation by admin

question and option management

quiz publishing

student attempts

auto evaluation

scoring and result storage

7. Subscription & Plan Module
Purpose
Controls which learning content and quizzes a student can access.

Why this module is needed
The platform is subscription based and must enforce content access using plans.

Main responsibilities
manage subscription plans

assign features to plans

activate and deactivate subscriptions

validate access before content delivery

8. Payment & Billing Module
Purpose
Manages paid subscriptions and payment reconciliation.

Why this module is needed
To support monetization of the platform.

Main responsibilities
Razorpay order creation

payment verification confirmation

invoice generation

payment history tracking

subscription activation after successful payment

9. Rewards & Referrals Module
Purpose
Improves user engagement and retention.

Why this module is needed
To incentivize daily learning activity and organic growth.

Main responsibilities
daily login rewards

referral code generation

referral reward tracking

reward wallet management

reward transaction history

10. Notification Module
Purpose
Delivers system and business notifications.

Why this module is needed
Students must be informed about:

new content

quiz availability

subscription expiry

rewards

announcements

Main responsibilities
in-app notifications

push event preparation

delivery status tracking

notification templates

11. Media Management Module
Purpose
Handles all images and files used by blogs, quizzes and learning content.

Why this module is needed
Binary data should not be stored in the database.

Main responsibilities
upload files to S3 / MinIO

generate secure URLs

maintain asset metadata

ownership and reference tracking

12. Activity & Audit Module
Purpose
Tracks user and admin actions.

Why this module is needed
For monitoring, compliance and debugging.

Main responsibilities
login history

admin operations

content publishing events

quiz attempts audit

payment and reward events

13. Reporting & Analytics Module
Purpose
Provides operational and business reports.

Why this module is needed
Admins need insight into:

active users

subscriptions

quiz performance

content popularity

revenue and rewards

sequenceDiagram
    participant Student
    participant Client
    participant Auth
    participant Subscription
    participant Content
    participant DB

    Student->>Client: Open learning content
    Client->>Auth: Validate token
    Auth-->>Client: OK
    
    Client->>Subscription: Check active plan
    Subscription->>DB: Validate subscription
    DB-->>Subscription: Active
    
    Client->>Content: Request post
    Content->>DB: Load markdown
    Content-->>Client: Renderable content

15. Module Interaction – Quiz Attempt Flow
sequenceDiagram
    participant Student
    participant Client
    participant Quiz
    participant Subscription
    participant DB

    Student->>Client: Start quiz
    Client->>Subscription: Access validation
    Subscription-->>Client: Allowed
    
    Client->>Quiz: Load quiz
    Quiz->>DB: Load questions
    Quiz-->>Client: Quiz data
    
    Student->>Client: Submit answers
    Client->>Quiz: Submit
    Quiz->>DB: Save answers and score
    Quiz-->>Client: Result

16. Module Interaction – Subscription Purchase Flow
sequenceDiagram
    participant Student
    participant Client
    participant Subscription
    participant Billing
    participant Razorpay
    participant DB

    Student->>Client: Buy plan
    Client->>Subscription: Create order
    Subscription->>Billing: Create payment request
    Billing->>Razorpay: Create order
    Razorpay-->>Billing: Order id
    Billing-->>Client: Payment details
    
    Student->>Client: Pay
    Client->>Billing: Verify payment
    Billing->>DB: Save payment
    Billing->>Subscription: Activate subscription

17. Module Interaction – Daily Login Reward Flow
sequenceDiagram
    participant Student
    participant Client
    participant Auth
    participant Reward
    participant Redis
    participant DB

    Student->>Client: Login
    Client->>Auth: Authenticate
    Auth->>Redis: Check daily reward flag
    Redis-->>Auth: Not rewarded
    
    Auth->>Reward: Create reward
    Reward->>DB: Save reward transaction
    Reward->>Redis: Mark rewarded

18. Module Boundaries (Clean Architecture)
┌─────────────────┐
│   Controller    │ (HTTP/REST layer)
├─────────────────┤
│  Application    │ (Service layer)
├─────────────────┤
│    Domain       │ (Business logic)
├─────────────────┤
│  Repository     │ (Data access)
└─────────────────┘

19. Future Module Extensions
This architecture allows future modules without redesign:

Module	Purpose
Trainer / Instructor	Course creation & student management
Institute / Organization	Team/group subscription management
Multi-tenant	White-labeled instances for enterprises
AI Recommendation	Personalized content suggestions
Content Personalization	Adaptive learning paths
20. Summary
The modular structure ensures:

✅ Clean separation of responsibilities
✅ Easier maintenance
✅ Scalable development
✅ Safe evolution into a SaaS training platform

Current Module Coverage
Domain Area	Modules
Access Control	Authentication, User Management
Learning	Content, Quiz, Media
Monetization	Subscription, Billing
Engagement	Rewards, Notification
Operations	Activity, Reporting
The current modules fully support:

Admin + Student roles

Web + Android clients

Subscription based learning

Content + quizzes + rewards + notifications