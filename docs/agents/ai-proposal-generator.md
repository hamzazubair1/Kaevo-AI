# Agent A3: AI Proposal Generator

> Transforms qualification data into polished, personalized service proposals.

---

## Identity

| Field | Value |
|-------|-------|
| **Name** | AI Proposal Generator |
| **ID** | `agent-a3-proposal-generator` |
| **Version** | 1.0.0 |
| **System** | System 1 — AI Agency Platform |
| **Type** | Generative |
| **Model** | GPT-4 (primary), Claude Opus (fallback) |
| **Trigger** | Qualification complete (Agent A2) or manual request |

---

## Purpose

Automatically generates comprehensive, branded service proposals from qualification data. Each proposal is tailored to the lead's industry, challenges, budget, and timeline — then queued for human review before delivery.

---

## Responsibilities

1. Receive qualification data from Agent A2 or admin input
2. Select the appropriate proposal template based on service type
3. Generate personalized content for each proposal section
4. Match relevant case studies from the portfolio database
5. Calculate pricing based on scope, complexity, and tier
6. Project ROI using industry benchmarks
7. Format proposal in branded HTML/PDF template
8. Queue for human review and approval
9. Track proposal views and engagement after delivery
10. Store proposal and metadata in the CRM

---

## Inputs

| Input | Source | Format |
|-------|--------|--------|
| Qualification data | CRM (from Agent A2) | JSON: industry, challenges, size, budget, timeline |
| Service catalog | Knowledge base | Service descriptions, features, pricing tiers |
| Case studies | Portfolio database | Matching case studies by industry/service |
| Industry benchmarks | Configuration DB | ROI projections, industry stats |
| Proposal templates | Template storage | HTML/Markdown branded templates |
| Company branding | Branding assets | Logos, colors, fonts |

---

## Outputs

| Output | Destination | Format |
|--------|-------------|--------|
| Draft proposal | Human review queue | HTML/Markdown |
| Final proposal | Lead via email | Branded PDF + web link |
| Proposal metadata | CRM Database | JSON: id, lead, amount, status, created_at |
| View tracking events | Analytics | Open, time-spent, section-viewed |
| Pricing breakdown | CRM Deal record | JSON: line items, total, payment schedule |

---

## Memory

### Short-Term
| Data | Purpose |
|------|---------|
| Current lead qualification data | Generate contextually relevant content |
| Selected template | Maintain template context during generation |

### Long-Term
| Data | Purpose | Storage |
|------|---------|---------|
| Proposal templates | Reusable structures per service type | Template DB |
| Case study index | Quick matching by industry/service | Portfolio DB |
| Industry benchmarks | ROI projection data | Config DB |
| Past proposals | Learn from accepted vs. rejected patterns | CRM |

### Shared
| Data | Shared With | Purpose |
|------|-------------|---------|
| Proposal document | Admin Dashboard, Email system | Review and delivery |
| Deal amount | CRM pipeline | Revenue forecasting |

---

## Tools

| Tool | Type | Purpose | Permissions |
|------|------|---------|-------------|
| CRM Read | Database API | Load qualification data | Read |
| CRM Write | Database API | Save proposal and deal data | Write |
| Portfolio Search | Database query | Find matching case studies | Read |
| Template Engine | Rendering service | Format proposal in brand template | Execute |
| PDF Generator | Conversion service | HTML → branded PDF | Execute |
| Email Service | Resend/SendGrid API | Deliver proposal to lead | Execute (after approval) |
| Tracking Pixel | Analytics service | Track opens and engagement | Read |

---

## Permissions

| Scope | Access | Approval Required |
|-------|--------|-------------------|
| Qualification data | Read | No |
| Proposal creation | Write | No |
| Pricing calculation | Execute | No |
| Proposal delivery to lead | Execute | **Yes — requires human approval** |
| Case study access | Read | No |
| Financial projections | Generate | No (based on benchmarks) |
| Template modification | None | N/A (admin only) |

---

## Generation Process

```
Qualification Data Received
       │
       ▼
Select proposal template
(based on primary service recommended)
       │
       ▼
Generate sections in parallel:
├── Executive Summary (personalized)
├── Current State Analysis (from pain points)
├── Proposed Solution (services mapped to gaps)
├── Implementation Plan (timeline from qualification)
├── Investment (pricing calculated from scope)
├── Case Studies (matched from portfolio)
└── Next Steps (booking link + contact)
       │
       ▼
Assemble into branded template
       │
       ▼
Calculate total pricing + payment schedule
       │
       ▼
Generate PDF version
       │
       ▼
Queue for human review
       │
       ├── Approved → Send to lead
       └── Needs edits → Return to admin
```

---

## Error Handling & Fallbacks

| Error Scenario | Response Strategy |
|----------------|-------------------|
| Incomplete qualification data | Generate with available data, flag gaps for admin |
| No matching case studies | Use industry-general examples, note for admin |
| Pricing outside standard range | Flag for manual pricing review |
| LLM generation failure | Retry → Use template defaults → Alert admin |
| PDF generation failure | Deliver HTML version, retry PDF async |

---

## Performance Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| Generation time | < 60 seconds | Timestamp delta |
| Proposals requiring edits | < 30% | Edited / total generated |
| Proposal acceptance rate | > 40% | Accepted / sent |
| Average deal size accuracy | Within ±15% | Proposal vs. final contract |
| View rate | > 80% | Opened / delivered |

---

## Configuration

```yaml
agent:
  id: "agent-a3-proposal-generator"
  name: "Kaevo AI Proposal Generator"
  version: "1.0.0"
  system: "system-1"
  type: "generative"

llm:
  primary:
    provider: "openai"
    model: "gpt-4"
    temperature: 0.5
    max_tokens: 4000
  fallback:
    provider: "anthropic"
    model: "claude-opus"

templates:
  default: "general-ai-services"
  available:
    - "ai-chatbot-proposal"
    - "workflow-automation-proposal"
    - "ai-voice-agent-proposal"
    - "custom-ai-solution-proposal"
    - "full-suite-proposal"

pricing:
  currency: "USD"
  include_payment_schedule: true
  discount_authority: "admin_only"

output:
  formats: ["html", "pdf"]
  branding: true
  tracking: true

review:
  require_human_approval: true
  auto_send_on_approval: true
  expiry_days: 14
```

---

> **Registry →** [Agent Registry](agent-registry.md) | **System →** [System 1](../architecture/system-1-agency-platform.md)
