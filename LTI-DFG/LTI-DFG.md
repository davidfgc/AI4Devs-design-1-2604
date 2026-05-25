## Description

LTI (Lead Talent Intelligence) is a next-generation Applicant Tracking System designed to streamline the end-to-end recruitment process for companies of all sizes. It centralizes job posting, candidate management, interview coordination, and hiring analytics in a single platform.

**Added value and competitive advantages:**
- **AI-assisted screening** — Automatically ranks and scores candidates based on job fit, reducing manual review time by up to 70%.
- **Collaborative hiring** — Real-time feedback, scorecards, and shared pipelines keep distributed hiring teams aligned without email threads.
- **Unified candidate experience** — A branded career portal and automated communications ensure every applicant receives timely, professional interactions.
- **Data-driven decisions** — Built-in analytics surface bottlenecks, source quality, and time-to-hire metrics so teams can continuously improve.
- **Seamless integrations** — Native connectors to LinkedIn, job boards, HRIS platforms, and calendar tools eliminate duplicate data entry.

**Main functions:**
1. Job posting and multi-channel distribution
2. Candidate application intake and resume parsing
3. Pipeline stage tracking and status management
4. Interview scheduling and feedback collection
5. Reporting and hiring metrics dashboard

---

## Lean Canvas

```mermaid
mindmap
  root((LTI ATS))
    Problem
      Fragmented hiring tools
      Slow manual screening
      Poor candidate experience
      Lack of hiring analytics
    Customer Segments
      SMBs scaling their teams
      Mid-market HR departments
      Recruitment agencies
    Unique Value Proposition
      AI-powered screening
      Collaborative real-time hiring
      Branded candidate experience
    Solution
      Centralized ATS platform
      Automated candidate ranking
      Integrated communications
      Analytics dashboard
    Channels
      Direct sales
      LinkedIn and HR communities
      Partner integrations
      Free trial / PLG motion
    Revenue Streams
      Monthly SaaS subscription
      Per-seat pricing
      Premium AI add-ons
    Cost Structure
      Engineering and infrastructure
      Sales and marketing
      Customer success
    Key Metrics
      Time-to-hire reduction
      Candidate NPS
      Pipeline conversion rate
      Monthly active recruiters
    Unfair Advantage
      Proprietary AI scoring model
      Deep HRIS integrations
      UX-first design
```

---

## Main Use Cases

### Use Case 1 — Post a Job and Collect Applications

A recruiter creates a new job opening in LTI, configures the requirements, and publishes it simultaneously to the company career page and external job boards. Candidates apply through a branded portal and their data is automatically parsed and stored.

```mermaid
sequenceDiagram
    actor Recruiter
    participant LTI
    participant JobBoards
    actor Candidate

    Recruiter->>LTI: Create job opening
    LTI->>LTI: Validate and save job details
    Recruiter->>LTI: Publish job
    LTI->>JobBoards: Distribute to LinkedIn, Indeed, etc.
    Candidate->>LTI: Submit application via career portal
    LTI->>LTI: Parse resume and extract structured data
    LTI-->>Recruiter: Notify new application received
```

---

### Use Case 2 — Screen and Advance Candidates Through the Pipeline

The hiring team reviews incoming applications, uses AI scoring to prioritize candidates, moves shortlisted profiles through pipeline stages, and collaborates via scorecards and comments.

```mermaid
sequenceDiagram
    actor Recruiter
    actor HiringManager
    participant LTI

    LTI->>LTI: AI scores and ranks new applications
    Recruiter->>LTI: Review ranked candidate list
    Recruiter->>LTI: Move candidate to "Phone Screen" stage
    Recruiter->>LTI: Add screening notes
    Recruiter->>LTI: Advance candidate to "Interview" stage
    LTI-->>HiringManager: Notify candidate ready for interview review
    HiringManager->>LTI: Submit scorecard and feedback
    HiringManager->>LTI: Approve or reject candidate
```

---

### Use Case 3 — Schedule Interviews and Collect Structured Feedback

Once a candidate is shortlisted, LTI coordinates interview scheduling between the candidate and interviewers, sends automated reminders, and collects structured feedback post-interview to support the final hiring decision.

```mermaid
sequenceDiagram
    actor Recruiter
    actor Interviewer
    actor Candidate
    participant LTI

    Recruiter->>LTI: Request interview for candidate
    LTI->>Candidate: Send availability request
    Candidate->>LTI: Submit preferred time slots
    LTI->>Interviewer: Propose confirmed interview slot
    Interviewer->>LTI: Accept slot
    LTI-->>Candidate: Send calendar invite and confirmation
    LTI-->>Interviewer: Send calendar invite and interview guide
    Interviewer->>LTI: Submit post-interview scorecard
    LTI-->>Recruiter: Notify feedback submitted
    Recruiter->>LTI: Review all feedback and make hiring decision
```

---

## High-Level System Design

LTI follows a **modular, service-oriented architecture** deployed on the cloud. The system is split into a frontend layer, a backend API layer, a set of specialized internal services, and a data layer — all communicating via a central API Gateway.

**Key architectural components:**

- **Web Application (SPA)** — React-based frontend consumed by recruiters, hiring managers, and candidates via browser.
- **API Gateway** — Single entry point that handles authentication, rate limiting, and request routing to internal services.
- **Core Services:**
  - *Job Service* — Manages job creation, publication, and distribution to external job boards via webhooks.
  - *Candidate Service* — Handles application intake, resume storage, and candidate profile management.
  - *Pipeline Service* — Tracks stage transitions, enforces workflow rules, and manages audit logs.
  - *Interview Service* — Coordinates scheduling, calendar sync, and reminder notifications.
  - *Notification Service* — Sends emails and in-app alerts triggered by system events.
  - *AI Scoring Service* — Runs candidate-to-job matching models and returns ranked scores asynchronously.
  - *Analytics Service* — Aggregates hiring metrics and serves reporting queries.
- **Data Layer:**
  - *Relational DB (PostgreSQL)* — Source of truth for jobs, candidates, pipelines, and users.
  - *Search Index (Elasticsearch)* — Powers fast candidate and job search queries.
  - *Object Storage (S3)* — Stores raw resumes and attachments.
  - *Cache (Redis)* — Reduces latency for frequently accessed data and session management.
- **External Integrations** — LinkedIn, Indeed, Google/Outlook Calendar, and HRIS platforms connected via outbound webhooks and OAuth flows.

```mermaid
graph TD
    subgraph Clients
        RC[Recruiter / Hiring Manager\nWeb App]
        CA[Candidate\nCareer Portal]
    end

    subgraph Gateway
        GW[API Gateway\nAuth · Rate Limiting · Routing]
    end

    subgraph Core Services
        JS[Job Service]
        CS[Candidate Service]
        PS[Pipeline Service]
        IS[Interview Service]
        NS[Notification Service]
        AI[AI Scoring Service]
        AS[Analytics Service]
    end

    subgraph Data Layer
        PG[(PostgreSQL)]
        ES[(Elasticsearch)]
        S3[(Object Storage S3)]
        RD[(Redis Cache)]
    end

    subgraph External
        JB[Job Boards\nLinkedIn · Indeed]
        CAL[Calendar\nGoogle · Outlook]
        HRIS[HRIS Platforms]
    end

    RC --> GW
    CA --> GW
    GW --> JS
    GW --> CS
    GW --> PS
    GW --> IS
    GW --> AS

    JS --> PG
    JS --> JB
    CS --> PG
    CS --> S3
    CS --> ES
    CS --> AI
    PS --> PG
    PS --> NS
    IS --> PG
    IS --> CAL
    IS --> NS
    AS --> PG
    AS --> RD
    AI --> PG

    NS --> CA
    NS --> RC
    HRIS --> GW
```

---

## C4 Diagrams

### C4 Level 1 — System Context

This diagram shows LTI as a single system and its relationships with the people and external systems that interact with it. It reveals the three distinct user roles — Recruiter, Hiring Manager, and Candidate — and makes clear that LTI sits at the center of an ecosystem of external job boards, calendar providers, and HRIS platforms. The key architectural decision visible here is that LTI acts as the single integration hub: all external touchpoints flow through it rather than being managed by individual teams in silos.

```mermaid
graph TD
    REC["👤 Recruiter\nCreates jobs, manages pipeline"]
    HM["👤 Hiring Manager\nReviews candidates, submits feedback"]
    CAN["👤 Candidate\nApplies for jobs"]

    LTI["🖥️ LTI ATS\nApplicant Tracking System"]

    JB["External: Job Boards\nLinkedIn · Indeed"]
    CAL["External: Calendar\nGoogle · Outlook"]
    HRIS["External: HRIS Platforms\nWorkday · BambooHR"]

    REC -->|Uses| LTI
    HM -->|Uses| LTI
    CAN -->|Applies via| LTI
    LTI -->|Posts jobs to| JB
    LTI -->|Syncs interviews with| CAL
    LTI -->|Syncs employee data with| HRIS
```

---

### C4 Level 2 — Containers

This diagram expands LTI into its deployable units, showing how the frontend, API Gateway, backend services, and data stores are structured and how they communicate. It makes visible the key decision to route all traffic through a single API Gateway — centralising authentication, rate limiting, and routing — rather than exposing services directly. It also shows the separation of concerns across services and which data stores each service owns.

```mermaid
graph TD
    REC["👤 Recruiter / Hiring Manager"]
    CAN["👤 Candidate"]

    subgraph LTI_ATS["LTI ATS"]
        SPA["Web App SPA\nReact · Browser"]
        PORTAL["Career Portal\nReact · Browser"]
        GW["API Gateway\nAuth · Routing · Rate Limiting"]

        subgraph Backend_Services["Backend Services"]
            JS["Job Service"]
            CS["Candidate Service"]
            PS["Pipeline Service"]
            IS["Interview Service"]
            NS["Notification Service"]
            AI["AI Scoring Service"]
            AS["Analytics Service"]
        end

        subgraph Data_Layer["Data Layer"]
            PG[("PostgreSQL\nPrimary DB")]
            ES[("Elasticsearch\nSearch Index")]
            S3_ST[("Object Storage S3\nResumes & Files")]
            RD[("Redis\nCache & Sessions")]
        end
    end

    JB2["Job Boards\nLinkedIn · Indeed"]
    CAL2["Calendar\nGoogle · Outlook"]
    HRIS2["HRIS Platforms"]

    REC -->|HTTPS| SPA
    CAN -->|HTTPS| PORTAL
    SPA -->|REST/JSON| GW
    PORTAL -->|REST/JSON| GW

    GW --> JS
    GW --> CS
    GW --> PS
    GW --> IS
    GW --> AS

    JS --> PG
    JS -->|Webhooks| JB2
    CS --> PG
    CS --> S3_ST
    CS --> ES
    CS --> AI
    PS --> PG
    PS --> NS
    IS --> PG
    IS -->|OAuth| CAL2
    IS --> NS
    AS --> PG
    AS --> RD
    HRIS2 -->|Webhooks| GW
```

---

### C4 Level 3 — Components (Backend: Candidate Service & Pipeline Service)

This diagram decomposes the two most central backend services into their internal components. The **Candidate Service** handles all application intake, resume processing, and AI-assisted scoring coordination; its internal separation between the Resume Parser, Profile Repository, and AI Scoring Client makes the data flow clear. The **Pipeline Service** owns stage transition logic and audit history, and delegates all outbound communication to the Notification Service. The key design decision visible here is that neither service handles concerns outside its bounded context — cross-service calls are narrow and explicit.

```mermaid
graph TD
    GW3["API Gateway"]

    subgraph Candidate_Service["Candidate Service"]
        CC["Candidate Controller\nREST handler"]
        CUC["Application Use Cases\nSubmit · Update · Search"]
        RP["Resume Parser\nExtracts structured data"]
        CPR["Candidate Repository\nDB read/write"]
        ASC["AI Scoring Client\nCalls AI Scoring Service"]
    end

    subgraph Pipeline_Service["Pipeline Service"]
        PC["Pipeline Controller\nREST handler"]
        PUC["Stage Use Cases\nAdvance · Reject · Hold"]
        WE["Workflow Engine\nEnforces stage rules"]
        PPR["Pipeline Repository\nDB read/write"]
        AUD["Audit Log Writer\nRecords all transitions"]
        NC["Notification Client\nCalls Notification Service"]
    end

    PG3[("PostgreSQL")]
    ES3[("Elasticsearch")]
    S3_C[("Object Storage S3")]
    AI3["AI Scoring Service"]
    NS3["Notification Service"]

    GW3 --> CC
    GW3 --> PC

    CC --> CUC
    CUC --> RP
    CUC --> CPR
    CUC --> ASC
    RP --> S3_C
    CPR --> PG3
    CPR --> ES3
    ASC --> AI3

    PC --> PUC
    PUC --> WE
    WE --> PPR
    WE --> AUD
    WE --> NC
    PPR --> PG3
    AUD --> PG3
    NC --> NS3
```

---

## Prompts

```
Refine my prompt:

"""
Eres un experto en producto, con experiencia en ATS (Applicant Tracking System).

¿Qué funcionalidades básicas tiene un ATS?

Descríbemelas en un listado en inglés, ordenado de mayor a menor prioridad
"""
```

```
  ## Objective
  List the core functionalities of a modern ATS, prioritized from most to least critical for a baseline product.

  ## Instructions
  1. Consider what features are essential for an ATS to be minimally viable and useful to recruiting teams.
  2. Order the list from highest to lowest priority based on business impact and frequency of use.
  3. For each item, provide a one-sentence description of what the feature does and why it matters.

  ## Constraints
  - Focus on core/basic functionalities only — exclude advanced or enterprise-tier features.
  - Keep descriptions concise (one sentence each).
  - Do not group features into categories; present a single flat ordered list.

  ## Output format
  A numbered list in English, ordered from highest to lowest priority:

  1. **Feature name** — Brief description of what it does and why it's essential.
  2. ...
```

```
we are goind to build an ATS called LTI

let's complete @LTI-DFG/LTI-DFG.md adding a section "Description" at the top, and the second one "Main use cases" which should
hace the 3 main ones 
```

```
we are goind to build an ATS called LTI

let's complete @LTI-DFG/LTI-DFG.md adding two sections at the top:

1 "Description" with: "Descripción breve del software LTI, valor añadido y ventajas competitivas. Explicación de las funciones
principales." 
2. "Lean Canvas": Añadir un diagrama Lean Canvas (en Mermaid con un mindmap) para entender el modelo de negocio", 
3. "Main use cases": "Descripción de los 3 casos de uso principales, con el diagrama en mermeid asociado a cada uno"
```

```
now after the use cases and before the prompts add a new section with:

"""
Diseño del sistema a alto nivel, tanto explicado como diagrama adjunto hecho en mermeid
"""
```

```
refine this prompt

now let's generate the C4 diagrams: context, containers, and components only the backend
```