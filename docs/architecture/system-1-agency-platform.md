# System 1 — AI Agency Platform (MVP)

> The complete inbound AI agency platform. From first website visit to signed client.

---

## System Purpose

System 1 is the **customer-facing platform** for Kaevo AI. It handles every step of the inbound client journey:

1. A visitor lands on the website
2. The AI Receptionist greets them
3. The Qualification Assistant determines fit
4. A meeting is booked automatically
5. A proposal is generated from qualification data
6. The client is onboarded via the Client Dashboard
7. The team manages everything via the Admin Dashboard

---

## Master User Flow

```
                    ┌─────────────────┐
                    │  Website Visitor │
                    └────────┬────────┘
                             │
                    ┌────────▼────────┐
                    │ Landing Website  │
                    │ (Pages + CTAs)   │
                    └────────┬────────┘
                             │
                    ┌────────▼────────┐
                    │ AI Receptionist  │
                    │ (Chat Widget)    │
                    │                  │
                    │ • Greet visitor  │
                    │ • Identify need  │
                    │ • Route intent   │
                    └────────┬────────┘
                             │
              ┌──────────────┼──────────────┐
              │              │              │
     ┌────────▼─────┐ ┌─────▼──────┐ ┌─────▼────────┐
     │ Browse Info   │ │ AI Qualif. │ │ Direct Book  │
     │ (Self-serve)  │ │ Assistant  │ │ (Calendar)   │
     └──────────────┘ └─────┬──────┘ └─────┬────────┘
                            │               │
                    ┌───────▼───────┐       │
                    │ Lead Scoring  │       │
                    │ & Fit Check   │       │
                    └───────┬───────┘       │
                            │               │
                     ┌──────▼───────────────▼──┐
                     │   Meeting Booking       │
                     │   (Calendar Integration)│
                     └──────────┬──────────────┘
                                │
                     ┌──────────▼──────────────┐
                     │   Proposal Generator    │
                     │   (Auto-generated from  │
                     │    qualification data)   │
                     └──────────┬──────────────┘
                                │
              ┌─────────────────┼─────────────────┐
              │                                   │
     ┌────────▼────────┐              ┌───────────▼──────────┐
     │ Admin Dashboard │              │  Client Dashboard    │
     │ (Internal)      │              │  (Client-facing)     │
     │                 │              │                      │
     │ • Pipeline      │              │ • Project status     │
     │ • Analytics     │              │ • Deliverables       │
     │ • CRM           │              │ • Communication      │
     │ • Team mgmt     │              │ • Invoices           │
     └─────────────────┘              └──────────────────────┘
```

---

## Component Architecture

### 1. Landing Website

**Purpose:** Convert visitors into conversations.

#### Pages

| Page | Purpose | Key Elements |
|------|---------|-------------|
| Home (`/`) | First impression + conversion | Hero with value prop, social proof, service cards, AI chat demo, CTA |
| Services (`/services`) | Detail offerings | Service cards with features, pricing tiers, comparison table |
| Services Detail (`/services/:slug`) | Deep-dive per service | Benefits, process, case studies, pricing, FAQ |
| Portfolio (`/portfolio`) | Demonstrate results | Case study grid, filterable by industry/service |
| About (`/about`) | Build trust | Story, team, values, tech stack |
| Blog (`/blog`) | SEO + authority | Article list, categories, search |
| Pricing (`/pricing`) | Transparent pricing | Tier comparison, feature matrix, FAQ |
| Contact (`/contact`) | Multiple contact methods | Form, calendar embed, email, chat |
| Book a Call (`/book`) | Direct scheduling | Embedded calendar (Cal.com/Calendly) |

#### Conversion Elements

| Element | Location | Purpose |
|---------|----------|---------|
| AI Chat Widget | Bottom-right, all pages | Instant engagement via AI Receptionist |
| CTA Buttons | Hero, section ends, sticky header | Drive to booking or chat |
| Exit Intent Popup | All pages (triggered on exit) | Capture leaving visitors |
| Lead Magnet | Blog, services pages | Email capture via valuable content |
| Social Proof Bar | Home, services | Trust signals (logos, testimonials, metrics) |

#### Technical Specifications

| Aspect | Specification |
|--------|--------------|
| Framework | Next.js 14+ (App Router, SSR/SSG) |
| Styling | Tailwind CSS + custom design system |
| Animation | Framer Motion |
| CMS | MDX for blog, Notion API for case studies |
| Analytics | Vercel Analytics + GA4 + Posthog |
| SEO | Dynamic meta, sitemap, schema.org, Open Graph |
| Performance | Lighthouse 95+, LCP < 2.5s, CLS < 0.1 |
| Hosting | Vercel (global CDN) |

---

### 2. AI Receptionist

**Purpose:** Greet every visitor, understand their needs, and route them to the right next step.

#### Conversation Flow

```
Visitor opens chat
       │
       ▼
"Hi! I'm Kaevo, your AI assistant.
 How can I help you today?"
       │
       ├── "I need AI for my business"
       │       │
       │       ▼
       │   Route → AI Qualification Assistant
       │
       ├── "What services do you offer?"
       │       │
       │       ▼
       │   Present service overview → Offer to qualify
       │
       ├── "I want to book a call"
       │       │
       │       ▼
       │   Route → Meeting Booking (direct)
       │
       ├── "Pricing?"
       │       │
       │       ▼
       │   Present tier overview → Offer qualification for custom quote
       │
       └── "Other / General question"
               │
               ▼
          Answer from knowledge base → Offer to connect with team
```

#### Features

| Feature | Description |
|---------|-------------|
| Intent Detection | Classify visitor intent from first message |
| Knowledge Base (RAG) | Answer questions from company docs, services, FAQ |
| Smart Routing | Route to qualification, booking, or info based on intent |
| Lead Capture | Collect name, email, company before handoff |
| Availability Awareness | Know business hours, team availability |
| Multi-language | Detect and respond in visitor's language |
| Conversation Memory | Remember context within the session |
| Human Handoff | Escalate to human when confidence is low or requested |
| Typing Indicators | Show "thinking" state for natural feel |

#### Widget Specifications

| Aspect | Specification |
|--------|--------------|
| Position | Bottom-right corner, all pages |
| Trigger | Auto-open after 5 seconds OR on user click |
| State | Minimized bubble → Expanded chat window |
| Persistence | Conversation persists across page navigation |
| Mobile | Full-screen on mobile, slide-up on tablet |
| Branding | Kaevo AI colors, avatar, custom greeting |
| Fallback | "Our team is reviewing your message" if AI fails |

> **Full agent spec →** [AI Receptionist Agent](../agents/ai-receptionist.md)

---

### 3. AI Qualification Assistant

**Purpose:** Determine if a visitor is a good fit for Kaevo AI's services and gather requirements for a proposal.

#### Qualification Flow

```
Handoff from Receptionist
       │
       ▼
"Let me ask a few questions to understand
 your business and find the best solution."
       │
       ▼
Q1: "What industry are you in?"
       │
       ▼
Q2: "What's the biggest challenge you're
     facing with [industry-specific pain]?"
       │
       ▼
Q3: "How large is your team?"
       │
       ▼
Q4: "What's your timeline for implementation?"
       │
       ▼
Q5: "Do you have a budget range in mind?"
       │
       ▼
Score lead → Generate qualification summary
       │
       ├── Score ≥ 70 → "Great fit! Let me book a strategy call."
       │                  → Route to Meeting Booking
       │
       ├── Score 40–69 → "I'd recommend starting with [service]."
       │                  → Present tailored recommendation + booking option
       │
       └── Score < 40 → "Here are some resources that might help."
                         → Provide self-serve content + newsletter signup
```

#### Lead Scoring Model

| Factor | Weight | Criteria |
|--------|--------|----------|
| Industry Fit | 25% | Is the industry in our target market? |
| Company Size | 15% | Team size and inferred revenue |
| Pain Severity | 25% | How urgent is the problem? |
| Budget Alignment | 20% | Does budget match our service tiers? |
| Timeline | 15% | How soon do they need a solution? |

#### Data Collected

| Field | Type | Required |
|-------|------|----------|
| Full Name | Text | Yes |
| Email | Email | Yes |
| Company Name | Text | Yes |
| Industry | Select | Yes |
| Team Size | Range | Yes |
| Primary Challenge | Text | Yes |
| Current Tools | Multi-select | No |
| Budget Range | Select | No |
| Timeline | Select | Yes |
| How They Found Us | Select | No |

> **Full agent spec →** [AI Qualification Assistant](../agents/ai-qualification-assistant.md)

---

### 4. Meeting Booking System

**Purpose:** Seamlessly schedule strategy calls between qualified leads and the Kaevo AI team.

#### Features

| Feature | Description |
|---------|-------------|
| Calendar Integration | Sync with Google Calendar / Outlook |
| Availability Detection | Show only available time slots |
| Timezone Handling | Auto-detect visitor timezone, convert accordingly |
| Meeting Types | 15-min intro, 30-min strategy, 60-min deep-dive |
| Pre-meeting Brief | Auto-generate brief from qualification data for the team |
| Reminders | Automated email + SMS reminders (24h, 1h before) |
| Rescheduling | Self-service reschedule/cancel link in confirmation |
| No-show Follow-up | Automated follow-up if lead doesn't attend |
| CRM Logging | Every booking recorded with qualification context |

#### Booking Flow

```
Lead qualified (Score ≥ 40)
       │
       ▼
Display available slots
(filtered by meeting type + team availability)
       │
       ▼
Lead selects date + time
       │
       ▼
Collect remaining info (phone number, agenda notes)
       │
       ▼
Confirmation email sent
       │
       ├── Include: Date, time, video link, agenda, "what to prepare"
       └── Internal: Notify team, create CRM entry, generate pre-meeting brief
       │
       ▼
Reminder sequence activates
       │
       ▼
Meeting occurs → Notes logged → Pipeline advances
```

#### Integration Points

| Integration | Purpose |
|-------------|---------|
| Cal.com / Calendly | Calendar scheduling engine |
| Google Calendar | Team availability sync |
| SendGrid / Resend | Confirmation + reminder emails |
| Twilio | SMS reminders (optional) |
| CRM Database | Log booking with full qualification context |

---

### 5. Proposal Generator

**Purpose:** Automatically create customized service proposals from qualification data.

#### Proposal Structure

```
┌─────────────────────────────────────────────┐
│           KAEVO AI PROPOSAL                  │
├─────────────────────────────────────────────┤
│                                              │
│  1. Executive Summary                        │
│     • Client overview                        │
│     • Key challenges identified              │
│     • Proposed solution summary              │
│                                              │
│  2. Current State Analysis                   │
│     • Business challenges                    │
│     • Manual processes identified            │
│     • Estimated cost of status quo           │
│                                              │
│  3. Proposed Solution                        │
│     • Recommended services                   │
│     • Architecture overview                  │
│     • Key features and capabilities          │
│                                              │
│  4. Implementation Plan                      │
│     • Phase breakdown                        │
│     • Timeline with milestones               │
│     • Deliverables per phase                 │
│                                              │
│  5. Investment                               │
│     • Pricing breakdown                      │
│     • Payment schedule                       │
│     • ROI projections                        │
│                                              │
│  6. Why Kaevo AI                             │
│     • Relevant case studies                  │
│     • Team expertise                         │
│     • Guarantees and support                 │
│                                              │
│  7. Next Steps                               │
│     • How to proceed                         │
│     • Timeline to kickoff                    │
│     • Contact information                    │
│                                              │
└─────────────────────────────────────────────┘
```

#### Generation Process

```
Qualification Data
       │
       ├── Industry → Select relevant case studies
       ├── Challenge → Map to service recommendations
       ├── Budget → Select appropriate pricing tier
       ├── Timeline → Generate realistic implementation plan
       └── Company Size → Adjust scope and complexity
       │
       ▼
LLM generates proposal sections
       │
       ▼
Template engine formats into branded PDF/HTML
       │
       ▼
Human review queue (admin reviews before sending)
       │
       ▼
Send to lead → Track opens and engagement
```

#### Features

| Feature | Description |
|---------|-------------|
| Auto-generation | Creates proposal from qualification + analysis data |
| Template System | Multiple templates per service type |
| Dynamic Pricing | Calculates pricing based on scope and complexity |
| Case Study Matching | Automatically selects relevant portfolio items |
| ROI Calculator | Projects return on investment based on industry benchmarks |
| Human Review | Proposals queue for admin approval before delivery |
| Tracking | Know when the lead opens and views the proposal |
| Versioning | Track proposal revisions and updates |
| PDF Export | Branded PDF generation for formal delivery |

> **Full agent spec →** [AI Proposal Generator](../agents/ai-proposal-generator.md)

---

### 6. CRM (Customer Relationship Management)

**Purpose:** Centralized database for all leads, clients, interactions, and pipeline management.

#### Pipeline Stages

```
┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐
│   New    │──▶│Qualified │──▶│ Meeting  │──▶│ Proposal │──▶│  Won /   │──▶│  Active  │
│   Lead   │   │   Lead   │   │ Booked   │   │   Sent   │   │   Lost   │   │  Client  │
└──────────┘   └──────────┘   └──────────┘   └──────────┘   └──────────┘   └──────────┘
```

#### CRM Entities

| Entity | Key Fields |
|--------|-----------|
| **Lead** | name, email, company, industry, source, score, status, assigned_to |
| **Company** | name, website, industry, size, location, tech_stack |
| **Interaction** | type (chat/email/call/meeting), content, timestamp, agent_id |
| **Proposal** | lead_id, version, status, amount, sent_date, opened_date |
| **Meeting** | lead_id, datetime, type, status, notes, recording_url |
| **Deal** | lead_id, service, amount, stage, probability, close_date |
| **Client** | company_id, contract_start, contract_end, services, mrr |

#### Features

| Feature | Description |
|---------|-------------|
| Pipeline View | Visual Kanban board of deal stages |
| Lead Timeline | Complete history of all interactions per lead |
| Activity Logging | Auto-log all AI and human interactions |
| Assignment | Assign leads to team members |
| Tagging | Custom tags for segmentation and filtering |
| Search & Filter | Full-text search across all entities |
| Import/Export | CSV import, API export |
| Notifications | Alerts for new leads, stage changes, follow-up reminders |

> **Full data model →** [Data Models](data-models.md)

---

### 7. Admin Dashboard

**Purpose:** Internal command center for the Kaevo AI team.

#### Dashboard Sections

| Section | Content |
|---------|---------|
| **Overview** | Key metrics (leads today, meetings this week, revenue pipeline, active clients) |
| **Pipeline** | Visual pipeline with drag-and-drop stage management |
| **Leads** | Lead list with search, filter, sort, and bulk actions |
| **Meetings** | Calendar view of upcoming meetings with pre-meeting briefs |
| **Proposals** | Proposal queue (pending review, sent, viewed, accepted, rejected) |
| **Clients** | Active client list with project status and health scores |
| **Analytics** | Conversion funnels, revenue trends, source attribution, agent performance |
| **AI Agents** | Agent status, conversation logs, performance metrics, configuration |
| **Team** | Team member management, roles, permissions |
| **Settings** | System configuration, integrations, billing |

#### Key Metrics Displayed

| Metric | Source |
|--------|--------|
| Leads This Week | CRM pipeline count |
| Qualification Rate | Qualified leads / Total leads |
| Booking Rate | Meetings booked / Qualified leads |
| Proposal Acceptance | Proposals accepted / Proposals sent |
| Average Deal Size | Sum of won deals / Count of won deals |
| Monthly Revenue | Current month closed revenue |
| Pipeline Value | Sum of open deal values × probability |
| AI Agent Accuracy | Successful resolutions / Total conversations |
| Client Health Score | Composite of engagement, satisfaction, renewal signals |

---

### 8. Client Dashboard

**Purpose:** Client-facing portal for transparency, communication, and project management.

#### Dashboard Sections

| Section | Content |
|---------|---------|
| **Overview** | Project summary, progress bar, next milestone, key contacts |
| **Projects** | List of active projects with status and deliverables |
| **Timeline** | Visual project timeline with milestones and completion status |
| **Deliverables** | File sharing, downloads, and version history |
| **Communication** | Message thread with the Kaevo AI team |
| **Meetings** | Past and upcoming meetings with recordings and notes |
| **Invoices** | Invoice history, payment status, upcoming payments |
| **Support** | AI-powered support chat + ticket system |
| **Reports** | Performance reports for deployed AI solutions |

#### Access Control

| Role | Permissions |
|------|------------|
| Client Admin | Full access, user management, billing |
| Client Member | View projects, deliverables, communication |
| Client Viewer | Read-only access to reports and status |

---

## Scalability Design

### Multi-Tenancy Preparation

Although V1 is a single-tenant agency platform, the architecture is designed for future multi-tenancy:

| Aspect | V1 (Single-tenant) | V3+ (Multi-tenant) |
|--------|--------------------|--------------------|
| Database | Single schema, one instance | Schema-per-tenant or row-level isolation |
| Auth | Single org, role-based | Org hierarchy, team-based permissions |
| AI Agents | Shared config | Per-tenant agent configuration |
| Branding | Kaevo AI only | White-label capable |
| Billing | Manual invoicing | Automated subscription billing |

### API-First Architecture

Every component communicates through well-defined APIs, enabling:
- Mobile app development without backend changes
- Third-party integrations via public API
- Microservice extraction when scaling demands it
- Agent-swappable architecture (replace any AI agent without system changes)

### Agent Modularity

Every AI agent is:
- **Configuration-driven** — behavior defined in YAML, not hard-coded
- **Model-agnostic** — can switch LLM providers via the LLM Gateway
- **Independently deployable** — can be updated without system downtime
- **Observable** — every action is logged and metriced

---

## Technology Stack (System 1)

| Layer | Technology |
|-------|-----------|
| Frontend | Next.js 14+, React 18, TypeScript, Tailwind CSS, Framer Motion |
| Backend API | FastAPI (Python) or Next.js API Routes |
| Database | PostgreSQL (via Supabase) |
| Cache | Redis |
| AI Agents | LangChain, OpenAI GPT-4, RAG with vector store |
| Vector Store | Pinecone (production), ChromaDB (development) |
| Auth | Supabase Auth (JWT + RBAC) |
| Email | Resend or SendGrid |
| Calendar | Cal.com API |
| File Storage | Supabase Storage or AWS S3 |
| Hosting | Vercel (frontend), Railway (backend) |
| Monitoring | Sentry, Vercel Analytics |

---

## Cross-References

| Document | Relevance |
|----------|-----------|
| [System Overview](system-overview.md) | How System 1 connects to System 2 |
| [System 2 — Outbound Engine](system-2-outbound-engine.md) | The outbound growth system |
| [Agent Registry](../agents/agent-registry.md) | All agent specifications |
| [Data Models](data-models.md) | Database schema details |
| [Security & Compliance](security-compliance.md) | Auth, data handling, privacy |

---

> _"System 1 is the front door. Every interaction — from first chat to signed contract — happens here."_
