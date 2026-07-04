# Kaevo AI — System Architecture Overview

> Master architecture document connecting all Kaevo AI systems, shared infrastructure, and evolution path.

---

## Systems at a Glance

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           KAEVO AI PLATFORM                                  │
├─────────────────────────────────┬───────────────────────────────────────────┤
│                                 │                                           │
│   SYSTEM 1                      │   SYSTEM 2                                │
│   AI Agency Platform (MVP)      │   AI Outbound Growth Engine (V2)          │
│                                 │                                           │
│   ┌─────────────────────┐       │   ┌──────────────────────────────┐       │
│   │ Landing Website      │       │   │ Business Discovery Agent     │       │
│   │ AI Receptionist      │       │   │ Website Analysis Agent       │       │
│   │ Qualification Asst.  │       │   │ Opportunity Scoring Agent    │       │
│   │ Meeting Booking      │       │   │ Personalized Proposal Agent  │       │
│   │ Proposal Generator   │       │   │ Outreach Email Agent         │       │
│   │ CRM                  │       │   │ CRM Integration              │       │
│   │ Admin Dashboard      │       │   │ Analytics Dashboard          │       │
│   │ Client Dashboard     │       │   └──────────────────────────────┘       │
│   └─────────────────────┘       │                                           │
│                                 │                                           │
├─────────────────────────────────┴───────────────────────────────────────────┤
│                        SHARED INFRASTRUCTURE                                 │
│                                                                              │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌────────────────────────┐│
│  │  Unified   │  │  Agent     │  │  Auth &    │  │  Monitoring &          ││
│  │  CRM DB    │  │  Runtime   │  │  Identity  │  │  Analytics             ││
│  └────────────┘  └────────────┘  └────────────┘  └────────────────────────┘│
│  ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌────────────────────────┐│
│  │  Vector    │  │  LLM       │  │  Queue &   │  │  Notification          ││
│  │  Store     │  │  Gateway   │  │  Workers   │  │  Service               ││
│  └────────────┘  └────────────┘  └────────────┘  └────────────────────────┘│
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## System Relationship

System 1 and System 2 operate as complementary halves of the Kaevo AI business engine:

| Dimension | System 1 — Agency Platform | System 2 — Outbound Engine |
|-----------|---------------------------|---------------------------|
| **Purpose** | Serve and convert inbound leads | Find and qualify outbound prospects |
| **Direction** | Inbound — clients come to us | Outbound — we go to clients |
| **Primary Users** | Website visitors, clients, admins | Internal sales team, AI agents |
| **Revenue Impact** | Direct client acquisition + delivery | Pipeline generation + lead supply |
| **Data Flow** | Visitor → Lead → Qualified → Client | Business → Analyzed → Scored → Outreach → Lead |
| **Shared Assets** | CRM, Proposal Engine, Meeting System | CRM, Proposal Engine, Meeting System |

### Data Flow Between Systems

```
SYSTEM 2 (Outbound)                    SYSTEM 1 (Inbound)
                                       
Business Discovery                     Landing Website
       │                                      │
       ▼                                      ▼
Website Analysis                       AI Receptionist
       │                                      │
       ▼                                      ▼
Opportunity Scoring                    AI Qualification
       │                                      │
       ▼                                      ▼
Proposal Generation ──────────────────► Proposal Generator
       │                                      │
       ▼                                      ▼
Email Outreach                         Meeting Booking
       │                                      │
       ▼                                      ▼
Reply / Interested ───────────────────► CRM (Unified)
                                              │
                                              ▼
                                       Client Dashboard
```

When an outbound prospect responds positively, they enter System 1's pipeline as a warm lead, inheriting all analysis data from System 2.

---

## Shared Infrastructure

### 1. Unified CRM Database

A single CRM serves both systems. Every business contact — whether inbound or outbound — is stored in one unified data model.

| Entity | System 1 Source | System 2 Source |
|--------|----------------|----------------|
| Business | Client registration | Business Discovery Agent |
| Contact | Form submission, chat | Discovery + enrichment |
| Interaction | Chat transcripts, meetings | Email outreach, follow-ups |
| Proposal | AI Proposal Generator | Personalized Proposal Agent |
| Pipeline Stage | Qualification flow | Scoring + outreach status |

### 2. Agent Runtime

All AI agents — from the Receptionist to the Outreach Email Agent — run on a shared agent runtime layer:

```
┌──────────────────────────────────────────────────┐
│                 Agent Runtime                     │
├──────────────────────────────────────────────────┤
│  ┌──────────┐  ┌──────────┐  ┌────────────────┐ │
│  │ LLM      │  │ Memory   │  │ Tool           │ │
│  │ Gateway  │  │ Manager  │  │ Registry       │ │
│  │          │  │          │  │                │ │
│  │ Routes   │  │ Short    │  │ API connectors │ │
│  │ to best  │  │ & Long   │  │ DB access      │ │
│  │ model    │  │ term     │  │ Web scrapers   │ │
│  └──────────┘  └──────────┘  └────────────────┘ │
├──────────────────────────────────────────────────┤
│  ┌──────────┐  ┌──────────┐  ┌────────────────┐ │
│  │ Guardrail│  │ Logging  │  │ Permission     │ │
│  │ Engine   │  │ & Audit  │  │ Enforcement    │ │
│  └──────────┘  └──────────┘  └────────────────┘ │
└──────────────────────────────────────────────────┘
```

### 3. LLM Gateway

A centralized LLM routing layer that all agents call. This enables:

- **Model flexibility** — swap GPT-4 for Claude or Gemini without changing agent code
- **Cost optimization** — route simple tasks to cheaper models, complex tasks to premium
- **Rate limiting** — centralized token budget management
- **Fallback chains** — automatic failover if a provider is down
- **Logging** — every LLM call is audited

| Task Complexity | Primary Model | Fallback | Cost Tier |
|----------------|--------------|----------|-----------|
| Simple (classification, extraction) | GPT-4o-mini | Gemini Flash | $ |
| Standard (conversation, analysis) | GPT-4o | Claude 3.5 Sonnet | $$ |
| Complex (proposals, deep analysis) | GPT-4 | Claude Opus | $$$ |
| Embeddings | text-embedding-3-small | — | $ |

### 4. Authentication & Identity

| Component | Purpose |
|-----------|---------|
| User Auth | JWT + refresh tokens for dashboard access |
| API Keys | Service-to-service communication |
| Role-Based Access | Admin, Sales, Client, Agent (machine) roles |
| Session Management | Redis-backed session store |

### 5. Queue & Workers

Asynchronous job processing for long-running tasks:

| Queue | Tasks |
|-------|-------|
| `agent-tasks` | AI agent executions (analysis, proposals, scoring) |
| `email-queue` | Outreach email preparation and scheduling |
| `analytics` | Dashboard metric computation |
| `webhooks` | External service callbacks |

### 6. Monitoring & Observability

| Layer | Tool | Purpose |
|-------|------|---------|
| Error Tracking | Sentry | Runtime error capture and alerting |
| Logging | Structured JSON logs | Audit trail, debugging |
| Metrics | Custom dashboard | Business KPIs, agent performance |
| Uptime | Health checks | Service availability monitoring |
| Cost Tracking | LLM usage dashboard | Token spend per agent, per client |

---

## Evolution Path

```
V1 (System 1)        V2 (System 1 + 2)       V3 (Platform)
─────────────        ──────────────────       ─────────────
Agency Website       + Outbound Engine        + Multi-tenant SaaS
AI Receptionist      + 5 Discovery Agents     + Client self-service
Meeting Booking      + Analytics Dashboard    + Agent Marketplace
Proposal Generator   + CRM Integration        + White-label option
Admin Dashboard      + Email Automation       + API for developers
Client Dashboard     + Scoring System         + Subscription billing
```

| Phase | System Scope | Architecture Style |
|-------|-------------|-------------------|
| V1 MVP | System 1 only | Monolith (Next.js + FastAPI) |
| V2 Growth | System 1 + System 2 | Service-oriented (shared DB) |
| V3 Platform | Full multi-tenant | Microservices (isolated DBs) |

---

## Cross-References

| Document | Purpose |
|----------|---------|
| [System 1 — Agency Platform](system-1-agency-platform.md) | Detailed MVP platform architecture |
| [System 2 — Outbound Engine](system-2-outbound-engine.md) | Detailed outbound growth engine architecture |
| [Agent Registry](../agents/agent-registry.md) | Master index of all AI agents |
| [Data Models](data-models.md) | Database schemas and entity relationships |
| [Security & Compliance](security-compliance.md) | Security architecture and compliance standards |
| [Enterprise Folder Structure](enterprise-folder-structure.md) | Repository organization |
| [Roadmap V2](../roadmap-v2.md) | 5-phase development roadmap |

---

> _"Two systems. One mission. Automate the entire sales cycle — from finding prospects to serving clients."_
