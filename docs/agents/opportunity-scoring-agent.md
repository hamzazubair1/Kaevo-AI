# Agent B3: Opportunity Scoring Agent

> Evaluates businesses based on automation gaps, industry fit, and firmographics to prioritize outreach.

---

## Identity

| Field | Value |
|-------|-------|
| **Name** | Opportunity Scoring Agent |
| **ID** | `agent-b3-opportunity-scoring` |
| **Version** | 1.0.0 |
| **System** | System 2 — AI Outbound Growth Engine |
| **Type** | Scoring / Analytical |
| **Model** | GPT-4o-mini (primary), Rule-engine (fallback) |
| **Trigger** | Website Analysis completed by Agent B2 |

---

## Purpose

To systematically evaluate and rank discovered businesses to ensure Kaevo AI focuses its outreach efforts on the highest-value prospects with the greatest likelihood of conversion.

---

## Responsibilities

1. Ingest the Website Analysis report (from B2) and Discovery profile (from B1).
2. Calculate scores across 6 distinct dimensions (Gap Severity, Industry Fit, Size, Tech Readiness, Engagement, Competition).
3. Compute a weighted composite score (0–100).
4. Classify the opportunity into tiers (Hot, Strong, Moderate, Low, Not a Fit).
5. Determine the next best action and outreach priority.
6. Route high-scoring businesses (≥ 60) to the Proposal generation queue.
7. Archive low-scoring businesses (< 60) for future re-evaluation.

---

## Inputs

| Input | Source | Format |
|-------|--------|--------|
| Business profile | CRM (from B1) | JSON: industry, location, size, source |
| Website analysis | CRM (from B2) | JSON: gaps, tech stack, performance |
| Scoring criteria | Configuration | YAML: weights and dimension rules |

---

## Outputs

| Output | Destination | Format |
|--------|-------------|--------|
| Opportunity Score | CRM Database | JSON: total score, dimension breakdown, classification |
| Proposal Queue Entry | Agent B4 queue | Event trigger for businesses ≥ 60 |
| Scoring Analytics | Analytics DB | Score distributions for dashboard |

---

## Memory

### Short-Term
| Data | Purpose |
|------|---------|
| Current business context | Calculate the score for the active business |

### Long-Term
| Data | Purpose | Storage |
|------|---------|---------|
| Scoring rules and weights | Define the scoring logic | Config DB |

### Shared
| Data | Shared With | Purpose |
|------|-------------|---------|
| Total Score | B4 (Proposal), B5 (Email), CRM | Prioritization and routing |
| Gap Severity | B4 (Proposal), CRM | Focus areas for proposals |

---

## Tools

| Tool | Type | Purpose | Permissions |
|------|------|---------|-------------|
| CRM Read | Database API | Load business and analysis data | Read |
| CRM Write | Database API | Save score and classification | Write |
| Queue Writer | Message queue | Add to Agent B4 queue if score ≥ 60 | Write |
| Rules Engine | Internal logic | Compute dimension scores | Execute |

---

## Permissions

| Scope | Access | Approval Required |
|-------|--------|-------------------|
| CRM data | Read / Write | No |
| Scoring rules | Read | No |
| Queue management | Write | No |

---

## Processing Logic

```
Analysis Report Received
       │
       ▼
Extract data points:
├── Gaps identified (count and severity)
├── Industry classification
├── Company size estimate
├── Technology stack signals
└── Engagement indicators (social links, reviews)
       │
       ▼
Calculate Dimension Scores (0-100 each):
├── Automation Gap Severity (30% weight)
├── Industry Fit (20% weight)
├── Company Size & Revenue (15% weight)
├── Technology Readiness (15% weight)
├── Engagement Signals (10% weight)
└── Competition Exposure (10% weight)
       │
       ▼
Compute Composite Score
Score = Σ (Dimension_Score × Dimension_Weight)
       │
       ▼
Classify Opportunity:
├── 80-100: Hot Opportunity
├── 60-79:  Strong Opportunity
├── 40-59:  Moderate Opportunity
├── 20-39:  Low Opportunity
└── 0-19:   Not a Fit
       │
       ▼
Save Score to CRM
       │
       ▼
Route based on score:
├── ≥ 60: Send to Agent B4 (Proposal Queue)
└── < 60: Archive and set re-evaluation date (90 days)
```

---

## Scoring Dimensions Details

(See System 2 Architecture for detailed dimension point tables).

---

## Error Handling & Fallbacks

| Error Scenario | Response Strategy |
|----------------|-------------------|
| Missing analysis data | Request re-analysis or use default (0) for missing dimension |
| Missing discovery data | Default to median score for unknown dimensions |
| LLM classification fails | Fallback to hardcoded rule-engine heuristics |

---

## Performance Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| Scoring completion rate | > 99% | Completed / queued |
| Scoring calibration | Score correlates with conversion | Ongoing statistical analysis |
| Average scoring time | < 5 seconds | Timestamp delta |

---

## Configuration

```yaml
agent:
  id: "agent-b3-opportunity-scoring"
  name: "Opportunity Scoring Agent"
  version: "1.0.0"
  system: "system-2"
  type: "analytical"

scoring:
  weights:
    gap_severity: 0.30
    industry_fit: 0.20
    company_size: 0.15
    tech_readiness: 0.15
    engagement: 0.10
    competition: 0.10
  
  routing_threshold: 60
  archive_revisit_days: 90
```
