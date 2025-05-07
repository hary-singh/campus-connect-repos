# Campus Connect – Backend Technical Architecture (Hybrid Microservices)

## Overview

Campus Connect uses a hybrid microservices architecture combining **Go** and **Java (GraalVM)** to balance **performance**, **developer velocity**, and **extensibility**.

Key technologies:
- **Go** for low-latency, high-throughput services (Auth, Gateway, Notification)
- **Java (GraalVM)** for complex, logic-heavy services (User, Student, Application)
- **gRPC** for internal service-to-service communication
- **Kafka** for async messaging and event-driven workflows
- **PostgreSQL** as the main relational database
- **OpenTelemetry** for observability and tracing
- **Podman/Docker Compose** for local development
- **Kubernetes** for scalable deployment

---

## Core Services and Responsibilities

### 1. **API Gateway (Go)**
- Handles all external traffic (web, mobile)
- Performs auth token validation
- Routes gRPC/REST calls to internal services
- Rate limiting and observability entry point

### 2. **Auth Service (Go)**
- Handles login, registration, and token issuance (JWT/OAuth)
- Stateless and horizontally scalable
- gRPC only

### 3. **User Service (Java - GraalVM)**
- Manages user profiles (students, admins, recruiters)
- Role-based access
- Handles email verification and updates

### 4. **Student Service (Java - GraalVM)**
- Manages academic info, resumes, eligibility
- GPA and department filters for jobs
- Integrates with Application and Job services

### 5. **Company & Job Service (Java - GraalVM)**
- Allows recruiters to post and manage job listings
- Enforces job eligibility criteria (min GPA, branch)
- Emits job-posted events to Kafka

### 6. **Application Service (Java - GraalVM)**
- Handles student job applications
- Tracks application status (pending, shortlisted, etc.)
- Subscribes to job events via Kafka for consistency

### 7. **Notification Service (Go)**
- Consumes Kafka events (e.g., application update)
- Sends email or in-app notifications to users
- Optimized for speed and throughput

---

## Data Flow & Communication

### Communication Protocols
- **gRPC** for internal service calls (low-latency, strongly typed)
- **Kafka** for event-driven updates and fan-out processing
- **REST** only at the API Gateway (for frontend compatibility)

### Example Flows

#### Student Applies to a Job
1. API Gateway → Application Service (gRPC)
2. Application Service → Kafka (`application_created`)
3. Notification Service consumes event → sends notification
4. Admin UI polls Application Service to see updates

#### Recruiter Posts a Job
1. API Gateway → Job Service
2. Job Service stores data in PostgreSQL
3. Job Service emits Kafka `job_posted` event
4. Student Service optionally subscribes to update views

---

## Data Storage

### PostgreSQL
- Used by all services
- Tables: `users`, `students`, `companies`, `jobs`, `applications`
- Can be partitioned/sharded per college in future

---

## Observability

### OpenTelemetry
- Distributed tracing across services
- Custom metrics (e.g., avg. application latency)
- Export to Prometheus/Grafana or Jaeger

---

## Local Dev & Deployment

- **Local**: Podman + Docker Compose (PostgreSQL, Kafka, gRPC services)
- **Production**: Kubernetes (scaling, health checks, rolling deploys)

---

## Extensibility

- Clean separation of concerns: Each service is single-responsibility
- Easily extendable by:
  - Adding new services (e.g., Interview Scheduling)
  - Publishing/consuming new Kafka topics
  - Extending protobuf/gRPC definitions
- Built for peak traffic (e.g., placement season), but low-cost when idle

