# Agent B4: Personalized Proposal Agent

> Generates highly customized AI automation recommendations based on website analysis.

---

## Identity

| Field | Value |
|-------|-------|
| **Name** | Personalized Proposal Agent |
| **ID** | `agent-b4-personalized-proposal` |
| **Version** | 1.0.0 |
| **System** | System 2 — AI Outbound Growth Engine |
| **Type** | Generative |
| **Model** | GPT-4 (primary), Claude Opus (fallback) |
| **Trigger** | Business scored ≥ 60 by Agent B3 |

---

## Purpose

To create tailored, compelling, and actionable AI automation recommendations for outbound prospects, demonstrating immediate value by mapping identified website gaps to specific Kaevo AI solutions.

---

## Responsibilities

1. Ingest website analysis (from B2) and opportunity score (from B3).
2. Map identified gaps to relevant Kaevo AI service offerings.
3. Select appropriate industry case studies and ROI benchmarks.
4. Generate a personalized outbound proposal document.
5. Create variable snippets for the Outreach Email Agent (B5).
6. Store the generated proposal in the CRM.

---

## Inputs

| Input | Source | Format |
|-------|--------|--------|
| Website Analysis | CRM (from B2) | JSON: gaps, tech stack |
| Business Profile | CRM (from B1) | JSON: industry, location, name |
| Service Mapping | Knowledge Base | YAML: Gap → Solution |
| ROI Benchmarks | Config DB | JSON: Expected savings by industry/solution |

---

## Outputs

| Output | Destination | Format |
|--------|-------------|--------|
| Custom Proposal | CRM / File Store | HTML/PDF document |
| Email Variables | CRM (for B5) | JSON: primary_gap, solution, estimated_savings |
| Email Queue Entry | Agent B5 Queue | Event trigger |

---

## Memory

### Short-Term
| Data | Purpose |
|------|---------|
| Current business context | Generate personalized content |

### Long-Term
| Data | Purpose | Storage |
|------|---------|---------|
| Proposal templates | Outbound-specific templates | Template DB |
| Service mappings | Logic for assigning solutions | Config DB |

### Shared
| Data | Shared With | Purpose |
|------|-------------|---------|
| Proposal Document | Sales Team (via CRM) | Reference during calls |
| Email Variables | Agent B5 (Email) | Crafting personalized outreach |

---

## Tools

| Tool | Type | Purpose | Permissions |
|------|------|---------|-------------|
| CRM Read | Database API | Load analysis and business data | Read |
| CRM Write | Database API | Save generated proposal and variables | Write |
| Template Engine | Rendering service | Format proposal (HTML/PDF) | Execute |
| Queue Writer | Message queue | Trigger Agent B5 | Write |

---

## Permissions

| Scope | Access | Approval Required |
|-------|--------|-------------------|
| Business data | Read | No |
| Proposal generation | Write | No |
| Trigger email drafting | Execute | No |

---

## Processing Logic

```
Opportunity Scored ≥ 60
       │
       ▼
Extract Gaps from Analysis (B2)
       │
       ▼
Map Gaps to Solutions:
(e.g., No chat widget → AI Receptionist)
       │
       ▼
Determine Primary Focus:
Identify the gap with the highest severity/impact
       │
       ▼
Select Industry Benchmarks & Case Studies
       │
       ▼
Generate Proposal Content:
├── Personalized Opening (acknowledging their site)
├── Gap Identification
├── Recommended AI Solutions
├── Expected ROI / Time Savings
└── Next Steps (Call to Action)
       │
       ▼
Render Document (HTML/PDF)
       │
       ▼
Extract Email Variables (primary_gap, recommended_service)
       │
       ▼
Save to CRM and Queue for B5 (Email Agent)
```

---

## Error Handling & Fallbacks

| Error Scenario | Response Strategy |
|----------------|-------------------|
| No clear gap mapping | Default to general AI consultation proposal |
| Missing industry benchmark | Use cross-industry averages |
| Generation timeout | Retry once, then alert admin |

---

## Performance Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| Proposal generation time | < 45 seconds | Timestamp delta |
| Output quality score | > 4.5/5.0 | Human review sample |
| Successful email variable extraction | > 95% | System audit |

---

## Configuration

```yaml
agent:
  id: "agent-b4-personalized-proposal"
  name: "Personalized Proposal Agent"
  version: "1.0.0"
  system: "system-2"
  type: "generative"

llm:
  primary:
    provider: "openai"
    model: "gpt-4"
    temperature: 0.4
    max_tokens: 2500

templates:
  default_outbound: "outbound-cold-proposal-v1"
```
