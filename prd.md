# Campus Connect – Product Requirements Document (PRD)

## 1. Overview
**Product Name**: Campus Connect  
**Purpose**: A SaaS platform to streamline campus placements by connecting students, colleges, and companies.  
**Goal**: Simplify and scale the end-to-end placement process with transparency and ease of use.

---

## 2. Problem Statement
- Placement processes are currently fragmented, manual, and high-pressure.
- Students lack visibility into job opportunities and application statuses.
- College placement teams manage data manually with spreadsheets.
- Recruiters face difficulty processing large volumes of applications efficiently.

---

## 3. Target Users
- **Students**: Primarily final and pre-final year students looking for internships or jobs.
- **College Admins**: Placement officers managing student records and company relations.
- **Recruiters/Companies**: HR teams hiring from multiple colleges/universities.

---

## 4. Core Features (MVP)

| Feature                        | Description                                                  | Priority |
|-------------------------------|--------------------------------------------------------------|----------|
| Student Registration           | Student signs up, verifies email, uploads resume             | High     |
| Job Listings                   | Companies post jobs, set eligibility (e.g., GPA filter)       | High     |
| Applications & Status Tracking| Students apply and view real-time status updates             | High     |
| Admin Dashboard                | Colleges manage and monitor student progress                 | Medium   |
| Notifications (In-app/email)  | Alerts for deadlines, new jobs, shortlists                   | Medium   |
| Authentication + RBAC         | Role-based access for students, admins, companies            | High     |
| Metrics Dashboard              | Insights for recruiters and colleges                         | Low      |

---

## 5. User Journeys

**Student Flow**:
1. Register and complete profile with resume
2. Browse job listings and filter by eligibility
3. Apply to eligible roles
4. Track application status

**College Admin Flow**:
1. Onboard students
2. Approve resumes
3. View job stats and placement reports

**Recruiter Flow**:
1. Create job postings by registering which college group they want to target
2. Filter eligible students based on GPA, skills, etc.
3. Review applications and shortlist candidates

---

## 6. Success Metrics

- % of eligible students who apply
- Job posting-to-hire conversion rate
- Time to shortlist & hire
- Retention of recruiters and colleges

---

## 7. Technical Architecture

- **Frontend**: React (Web), Swift & Kotlin (Mobile)
- **Backend**: 
  - Go: High-performance services (API Gateway, Auth, Notifications)
  - Java (GraalVM): Business-heavy services (User, Application, Student, Company)
- **Communication**: gRPC + Kafka
- **Database**: PostgreSQL (Free, ACID-compliant, scalable with partitioning)
- **Observability**: OpenTelemetry (metrics + traces)
- **Local Dev**: Podman + Docker Compose
- **Deployment**: Kubernetes (scalable, auto-healing) on AWS EKS

---

## 8. Risks & Mitigations

| Risk                                    | Mitigation Strategy                                              |
|----------------------------------------|------------------------------------------------------------------|
| Peak load during placement season      | Kafka buffering, autoscaling, stateless services                 |
| Privacy and data compliance            | Role-based access, encrypted storage, audit logging              |
| Adoption friction with colleges        | Low-friction onboarding, CSV import along with gpa import option |

