# Agent A2: AI Qualification Assistant

> Turns conversations into qualified leads with structured data for proposals and meetings.

---

## Identity

| Field | Value |
|-------|-------|
| **Name** | AI Qualification Assistant |
| **ID** | `agent-a2-qualification` |
| **Version** | 1.0.0 |
| **System** | System 1 — AI Agency Platform |
| **Type** | Conversational |
| **Model** | GPT-4o (primary), Claude 3.5 Sonnet (fallback) |
| **Trigger** | Handoff from Agent A1 (Receptionist) |

---

## Purpose

The Qualification Assistant conducts a structured yet natural conversation to determine if a lead is a good fit for Kaevo AI's services. It collects requirements, scores the lead, and routes them to meeting booking or alternative next steps.

---

## Responsibilities

1. Receive handoff context from the AI Receptionist (name, email, initial intent)
2. Conduct a natural, structured qualification conversation (5–8 questions)
3. Adapt questions dynamically based on previous answers
4. Score the lead on a 0–100 scale using the qualification model
5. Collect structured data for proposal generation
6. Route high-score leads (≥ 70) to meeting booking
7. Provide recommendations for mid-score leads (40–69)
8. Offer resources for low-score leads (< 40)
9. Store all qualification data in the CRM
10. Generate a qualification summary for the sales team

---

## Inputs

| Input | Source | Format |
|-------|--------|--------|
| Handoff context | Agent A1 | JSON: name, email, company, intent, transcript |
| Visitor responses | Chat widget | Text strings |
| Industry benchmarks | Configuration DB | JSON: pain points, typical scope, pricing |
| Service catalog | Knowledge base | Service descriptions and tiers |
| Conversation history | Session memory | Message array |

---

## Outputs

| Output | Destination | Format |
|--------|-------------|--------|
| Qualification score | CRM + routing logic | Integer: 0–100 |
| Lead profile | CRM Database | Structured JSON |
| Qualification summary | Admin Dashboard | Markdown report |
| Booking trigger | Meeting Booking System | Event with lead context |
| Proposal request | Agent A3 | Event with qualification data |
| Conversation log | CRM + Analytics | Structured interaction record |

---

## Memory

### Short-Term (Session)
| Data | Purpose |
|------|---------|
| Handoff context from A1 | Avoid re-asking known info |
| Questions asked and answers | Track qualification progress |
| Running score calculation | Determine routing mid-conversation |
| Detected pain points | Inform follow-up questions |

### Long-Term (Persistent)
| Data | Purpose | Storage |
|------|---------|---------|
| Industry question templates | Dynamic question selection | Configuration DB |
| Successful qualification patterns | Optimize conversion rates | Analytics DB |
| Service-to-pain-point mappings | Accurate recommendations | Knowledge base |

### Shared (Cross-Agent)
| Data | Shared With | Purpose |
|------|-------------|---------|
| Qualification summary | Agent A3 (Proposal Gen), Admin Dashboard | Proposal input, team briefing |
| Lead score + profile | CRM, Meeting Booking System | Routing and prioritization |

---

## Tools

| Tool | Type | Purpose | Permissions |
|------|------|---------|-------------|
| CRM Read | Database API | Check for existing lead data | Read |
| CRM Write | Database API | Save qualification data | Write |
| Score Calculator | Internal function | Compute lead score from answers | Execute |
| Booking Trigger | Internal event | Initiate meeting scheduling | Execute |
| Proposal Request | Internal event | Trigger proposal generation | Execute |
| Industry Benchmarks | Configuration DB | Load industry-specific questions | Read |

---

## Permissions

| Scope | Access | Approval Required |
|-------|--------|-------------------|
| Lead data | Read / Write | No |
| Qualification data | Write | No |
| Meeting booking trigger | Execute | No (visitor consents) |
| Proposal generation trigger | Execute | No (auto on qualification) |
| Service pricing | Read | No |
| Financial data | None | N/A |
| Admin settings | None | N/A |

---

## Qualification Flow

```
Handoff from Receptionist (with name, email, intent)
       │
       ▼
"Thanks {name}! Let me learn about your business
 so I can recommend the best solution."
       │
       ▼
Q1: "What industry is {company} in?"
       │ → Loads industry-specific follow-ups
       ▼
Q2: "What's the biggest operational challenge
     you're facing right now?"
       │ → Maps to pain points and services
       ▼
Q3: "How is your team currently handling
     {pain_point}?"
       │ → Assesses current state
       ▼
Q4: "How many people are on your team?"
       │ → Informs scope and pricing
       ▼
Q5: "What's your timeline — when would you
     ideally have a solution in place?"
       │ → Urgency scoring
       ▼
Q6: "Do you have a budget range in mind?"
       │ → Budget alignment (optional, skip if awkward)
       ▼
Calculate score → Generate summary
       │
       ├── Score ≥ 70: "Great fit! Let me find a time
       │                 for a strategy call."
       │                 → Trigger booking
       │
       ├── Score 40–69: "Based on what you've shared,
       │                  I'd recommend starting with
       │                  {service}. Want to discuss?"
       │                  → Offer booking or info
       │
       └── Score < 40: "Here are some resources that
                        might help at your stage."
                        → Provide content + nurture signup
```

---

## Lead Scoring Model

| Factor | Weight | Scoring Criteria |
|--------|--------|-----------------|
| **Industry Fit** | 25% | Tier 1 industries: 90+, Tier 2: 70, Tier 3: 50, Tier 4: 30 |
| **Pain Severity** | 25% | Critical + urgent: 90+, Important: 70, Nice-to-have: 40 |
| **Budget Alignment** | 20% | Matches service tier: 90+, Flexible: 70, Unclear: 50, Low: 20 |
| **Team Size** | 15% | 20+: 90, 10–20: 70, 5–10: 50, 1–5: 30 |
| **Timeline** | 15% | Immediate: 90, 1–3 months: 70, 3–6 months: 50, Exploring: 30 |

---

## Error Handling & Fallbacks

| Error Scenario | Response Strategy |
|----------------|-------------------|
| Visitor gives vague answers | Rephrase question with examples |
| Visitor skips questions | Mark as optional, continue with available data |
| Visitor wants to skip qualification | Offer direct booking as alternative |
| Visitor asks unrelated questions | Brief answer → redirect to qualification |
| Budget question is uncomfortable | Skip gracefully → "No worries, we can discuss that later" |
| LLM timeout | Retry once → "Let me save your info, our team will reach out" |

---

## Performance Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| Qualification completion rate | > 75% | Completed / started |
| Average qualification time | < 4 minutes | Timestamp delta |
| Score accuracy | > 85% | Score vs. actual conversion (retroactive) |
| Booking conversion (score ≥ 70) | > 60% | Bookings / high-score leads |
| Data completeness | > 90% | Fields filled / required fields |
| Visitor satisfaction | > 4.0/5.0 | Post-qualification rating |

---

## Configuration

```yaml
agent:
  id: "agent-a2-qualification"
  name: "Kaevo AI Qualification Assistant"
  version: "1.0.0"
  system: "system-1"
  type: "conversational"

llm:
  primary:
    provider: "openai"
    model: "gpt-4o"
    temperature: 0.3
    max_tokens: 400
  fallback:
    provider: "anthropic"
    model: "claude-3-5-sonnet"

scoring:
  model: "weighted_linear"
  threshold_high: 70
  threshold_medium: 40
  weights:
    industry_fit: 0.25
    pain_severity: 0.25
    budget_alignment: 0.20
    team_size: 0.15
    timeline: 0.15

questions:
  max_questions: 8
  min_questions: 4
  allow_skip: true
  budget_question: "optional"

routing:
  high_score_action: "booking"
  medium_score_action: "recommendation"
  low_score_action: "resources"

guardrails:
  max_conversation_turns: 20
  timeout_minutes: 10
  fallback_action: "save_and_notify_team"
```

---

> **Registry →** [Agent Registry](agent-registry.md) | **System →** [System 1](../architecture/system-1-agency-platform.md)
