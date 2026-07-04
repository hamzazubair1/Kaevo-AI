# Kaevo AI — AI Agent Registry

> Master index of all AI agents across System 1 and System 2. Every agent's identity, classification, and dependency map.

---

## Agent Inventory

### System 1 — AI Agency Platform (Inbound)

| # | Agent | Type | Purpose | Priority |
|---|-------|------|---------|----------|
| A1 | [AI Receptionist](ai-receptionist.md) | Conversational | Greet visitors, detect intent, route to next step | P0 — MVP |
| A2 | [AI Qualification Assistant](ai-qualification-assistant.md) | Conversational | Qualify leads through structured conversation | P0 — MVP |
| A3 | [AI Proposal Generator](ai-proposal-generator.md) | Generative | Create service proposals from qualification data | P1 — MVP |

### System 2 — AI Outbound Growth Engine

| # | Agent | Type | Purpose | Priority |
|---|-------|------|---------|----------|
| B1 | [Business Discovery Agent](business-discovery-agent.md) | Research | Find target businesses by industry/location | P0 — V2 |
| B2 | [Website Analysis Agent](website-analysis-agent.md) | Analysis | Crawl websites, identify automation gaps | P0 — V2 |
| B3 | [Opportunity Scoring Agent](opportunity-scoring-agent.md) | Scoring | Score businesses on opportunity value | P0 — V2 |
| B4 | [Personalized Proposal Agent](personalized-proposal-agent.md) | Generative | Create custom AI recommendations per business | P1 — V2 |
| B5 | [Outreach Email Agent](outreach-email-agent.md) | Generative | Draft personalized outreach email sequences | P1 — V2 |

### Future Agents (Phase 3+)

| # | Agent | Type | Purpose | Phase |
|---|-------|------|---------|-------|
| C1 | Internal Task Agent | Operational | Automate internal Kaevo AI operations | Phase 3 |
| C2 | Client Reporting Agent | Analytical | Generate client performance reports | Phase 4 |
| C3 | Content Generation Agent | Generative | Create blog posts, social content | Phase 3 |
| C4 | Invoice & Billing Agent | Operational | Automate invoicing and payment tracking | Phase 3 |
| C5 | Onboarding Agent | Conversational | Guide new clients through setup | Phase 4 |

---

## Agent Dependency Map

```
SYSTEM 1 (Inbound Flow)
═══════════════════════

 Website Visitor
       │
       ▼
┌──────────────┐
│ A1: AI       │
│ Receptionist │─────────────┐
└──────┬───────┘             │
       │                     │
       ▼                     ▼
┌──────────────┐     ┌──────────────┐
│ A2: Qualif.  │     │ Direct       │
│ Assistant    │     │ Booking      │
└──────┬───────┘     └──────────────┘
       │
       ▼
┌──────────────┐
│ A3: Proposal │
│ Generator    │
└──────────────┘


SYSTEM 2 (Outbound Flow)
═════════════════════════

┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ B1: Business │────▶│ B2: Website  │────▶│ B3: Scoring  │
│ Discovery    │     │ Analysis     │     │ Agent        │
└──────────────┘     └──────────────┘     └──────┬───────┘
                                                  │
                                           Score ≥ 60
                                                  │
                                                  ▼
                     ┌──────────────┐     ┌──────────────┐
                     │ B5: Outreach │◀────│ B4: Personal.│
                     │ Email Agent  │     │ Proposal     │
                     └──────┬───────┘     └──────────────┘
                            │
                     Reply → System 1
```

---

## Shared Infrastructure

All agents connect to shared services:

```
┌─────────────────────────────────────────────────┐
│              SHARED AGENT SERVICES               │
├─────────────────────────────────────────────────┤
│                                                  │
│  ┌───────────┐  ┌───────────┐  ┌─────────────┐ │
│  │ LLM       │  │ Memory    │  │ Tool        │ │
│  │ Gateway   │  │ Manager   │  │ Registry    │ │
│  └───────────┘  └───────────┘  └─────────────┘ │
│                                                  │
│  ┌───────────┐  ┌───────────┐  ┌─────────────┐ │
│  │ CRM       │  │ Logging   │  │ Permission  │ │
│  │ Database  │  │ & Audit   │  │ Engine      │ │
│  └───────────┘  └───────────┘  └─────────────┘ │
│                                                  │
└─────────────────────────────────────────────────┘
```

| Service | Used By | Purpose |
|---------|---------|---------|
| LLM Gateway | All agents | Centralized model routing and cost management |
| Memory Manager | A1, A2, B2, B4, B5 | Short-term conversation and long-term knowledge |
| Tool Registry | All agents | API connectors, database access, web tools |
| CRM Database | All agents | Read/write business and lead data |
| Logging & Audit | All agents | Every action recorded for compliance |
| Permission Engine | All agents | Enforce read/write scope and approval gates |

---

## Agent Specification Template

Every agent document follows this exact structure:

```markdown
# Agent Name

## Identity
- Name, ID, version, system, type

## Purpose
- What this agent does and why it exists

## Responsibilities
- Enumerated list of duties

## Inputs
- Data sources, triggers, upstream dependencies

## Outputs
- Deliverables, downstream targets, side effects

## Memory
- Short-term (conversation/session)
- Long-term (persistent knowledge)
- Shared (cross-agent accessible)

## Tools
- APIs, databases, external services the agent can use

## Permissions
- Read scope, write scope, human approval gates

## Conversation Flow / Processing Logic
- Step-by-step operational logic

## Error Handling & Fallbacks
- Failure modes and recovery strategies

## Performance Metrics
- KPIs for measuring agent effectiveness

## Configuration
- YAML configuration example
```

---

## Agent Model Assignments

| Agent | Primary Model | Fallback | Reasoning |
|-------|--------------|----------|-----------|
| A1: Receptionist | GPT-4o | Claude 3.5 Sonnet | Fast, conversational, good intent detection |
| A2: Qualification | GPT-4o | Claude 3.5 Sonnet | Structured data extraction + conversation |
| A3: Proposal Gen | GPT-4 | Claude Opus | Long-form generation, requires depth |
| B1: Discovery | GPT-4o-mini | Gemini Flash | Simple extraction and classification |
| B2: Analysis | GPT-4o | Claude 3.5 Sonnet | Complex analysis of scraped data |
| B3: Scoring | GPT-4o-mini | — | Rule-based with light LLM augmentation |
| B4: Proposal | GPT-4 | Claude Opus | Detailed, personalized content generation |
| B5: Email | GPT-4o | Claude 3.5 Sonnet | Persuasive, natural language generation |

---

## Cost Estimation (per 100 businesses processed)

| Agent | Calls | Avg Tokens | Estimated Cost |
|-------|-------|-----------|----------------|
| B1: Discovery | 100 | 500 in / 200 out | $0.50 |
| B2: Analysis | 100 | 2,000 in / 1,000 out | $3.00 |
| B3: Scoring | 100 | 800 in / 200 out | $0.30 |
| B4: Proposal | 60 | 1,500 in / 2,000 out | $4.20 |
| B5: Email | 60 | 1,000 in / 500 out | $1.50 |
| **Total** | — | — | **~$9.50 per 100 businesses** |

> **Cost per qualified lead: ~$0.16** (assuming 60% pass scoring threshold)

---

> _"Each agent is a specialist. Together, they form the Kaevo AI workforce."_
