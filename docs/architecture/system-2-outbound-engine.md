# System 2 — AI Outbound Growth Engine (Version 2)

> A fully autonomous outbound sales pipeline powered by specialized AI agents. Discover → Analyze → Score → Propose → Outreach → Convert.

---

## System Purpose

System 2 is the **proactive growth engine** for Kaevo AI. Instead of waiting for leads, this system:

1. **Discovers** businesses that could benefit from AI automation
2. **Analyzes** their websites for automation gaps
3. **Scores** each opportunity by value and fit
4. **Generates** personalized AI automation proposals
5. **Prepares** compliant outreach emails for human review
6. **Tracks** the entire outbound pipeline from discovery to conversion

---

## Master Pipeline Flow

```
┌─────────────────────────────────────────────────────────────────────────┐
│                     AI OUTBOUND GROWTH ENGINE                            │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  ┌──────────────┐     ┌──────────────┐     ┌──────────────────────┐    │
│  │  DISCOVERY   │────▶│   ANALYSIS   │────▶│     SCORING          │    │
│  │              │     │              │     │                      │    │
│  │ Find target  │     │ Crawl site,  │     │ Multi-factor         │    │
│  │ businesses   │     │ identify     │     │ opportunity score    │    │
│  │ by industry, │     │ automation   │     │ (0–100)              │    │
│  │ location,    │     │ gaps and     │     │                      │    │
│  │ keywords     │     │ opportunities│     │ Rank and prioritize  │    │
│  └──────────────┘     └──────────────┘     └──────────┬───────────┘    │
│                                                        │                │
│                                            Score ≥ 60  │  Score < 60   │
│                                                        │  → Archive    │
│                                                        ▼                │
│  ┌──────────────────────┐     ┌──────────────────────────────────┐     │
│  │  OUTREACH             │◀────│  PERSONALIZED PROPOSAL           │     │
│  │                       │     │                                  │     │
│  │  Draft email          │     │  Generate custom AI automation   │     │
│  │  sequence for         │     │  recommendations based on        │     │
│  │  human review         │     │  analysis findings               │     │
│  │                       │     │                                  │     │
│  │  Send on approval     │     │  Map gaps → solutions → ROI     │     │
│  └───────────┬───────────┘     └──────────────────────────────────┘     │
│              │                                                          │
│              ▼                                                          │
│  ┌──────────────────────┐     ┌──────────────────────────────────┐     │
│  │  CRM INTEGRATION     │────▶│  ANALYTICS DASHBOARD             │     │
│  │                       │     │                                  │     │
│  │  Store everything:    │     │  Track everything:               │     │
│  │  business, contact,   │     │  discoveries, analyses,          │     │
│  │  analysis, outreach,  │     │  emails, replies, meetings,      │     │
│  │  meeting, status      │     │  conversions, revenue            │     │
│  └───────────────────────┘     └──────────────────────────────────┘     │
│                                                                          │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## Component 1: Business Discovery Agent

### Purpose
Find publicly available businesses that match Kaevo AI's ideal customer profile based on industries, locations, or keywords.

### Discovery Methods

| Method | Source | Data Extracted |
|--------|--------|---------------|
| Google Maps API | Location-based search | Business name, address, phone, website, rating, reviews |
| Google Search | Keyword-based search | Company websites, LinkedIn pages, directories |
| Industry Directories | Niche directories (Yelp, Clutch, G2) | Company profiles, categorization |
| LinkedIn (Public) | Company pages | Size, industry, description, key personnel |
| Social Media | Twitter/X, Facebook, Instagram | Online presence, engagement |

### Search Parameters

| Parameter | Type | Example |
|-----------|------|---------|
| Industry | Category | "dental clinics", "real estate agencies", "law firms" |
| Location | Geography | "New York, NY", "London, UK", "radius:50km" |
| Keywords | Search terms | "no chatbot", "contact us form", "call to book" |
| Company Size | Range | "10-50 employees", "small business" |
| Tech Stack | Detection | "WordPress", "Shopify", "no custom CRM" |

### Output per Business

```yaml
business:
  name: "Sunrise Dental Clinic"
  website: "https://sunrisedental.com"
  industry: "Healthcare / Dental"
  location: "Austin, TX, USA"
  phone: "+1-512-555-0123"
  email: "info@sunrisedental.com"  # public only
  size_estimate: "10-25 employees"
  online_presence:
    google_rating: 4.2
    review_count: 187
    social_media:
      facebook: true
      instagram: true
      linkedin: false
  discovered_at: "2026-07-04T12:00:00Z"
  source: "google_maps"
  status: "pending_analysis"
```

### Daily Capacity

| Tier | Businesses/Day | Notes |
|------|---------------|-------|
| Conservative | 50 | Manual review of all results |
| Standard | 200 | AI-filtered with spot checks |
| Aggressive | 500+ | Fully automated with quality thresholds |

> **Full agent spec →** [Business Discovery Agent](../agents/business-discovery-agent.md)

---

## Component 2: Website Analysis Agent

### Purpose
Analyze each discovered business's website and identify specific automation opportunities, technology gaps, and improvement areas.

### Analysis Dimensions

#### 1. Customer Communication Gaps

| Gap | Detection Method | Opportunity |
|-----|-----------------|-------------|
| **No chatbot** | Scan for chat widget scripts (Intercom, Drift, Tidio, etc.) | AI Chatbot deployment |
| **No live chat** | Check for real-time communication tools | 24/7 AI support agent |
| **Contact form only** | Detect forms without instant response | AI-powered instant response |
| **Phone-only contact** | Only phone number, no digital channels | AI Voice Agent + chat |
| **Slow response indicators** | "We'll get back to you in 24-48h" type messaging | Instant AI response |

#### 2. Lead Capture Gaps

| Gap | Detection Method | Opportunity |
|-----|-----------------|-------------|
| **No lead magnet** | Scan for gated content, email capture | AI content + lead capture |
| **Basic contact form** | Only name/email/message fields | Smart qualification form |
| **No popup/CTA** | Missing conversion optimization | AI-powered engagement |
| **No booking system** | No calendar integration detected | AI meeting scheduling |
| **Manual quote process** | "Request a quote" with no automation | AI proposal generator |

#### 3. Operational Gaps

| Gap | Detection Method | Opportunity |
|-----|-----------------|-------------|
| **Manual booking** | "Call to schedule" messaging | AI scheduling assistant |
| **No FAQ section** | Missing self-service information | AI knowledge base |
| **No help center** | No support documentation | AI-powered support |
| **Manual intake forms** | Paper or basic form processes | AI intake automation |
| **No client portal** | No login/dashboard for clients | Client portal development |

#### 4. Technology Assessment

| Signal | Detection Method | Implication |
|--------|-----------------|-------------|
| **CMS type** | Wappalyzer-style tech detection | Integration complexity |
| **Mobile responsiveness** | Viewport and responsive checks | Technical maturity |
| **SSL certificate** | HTTPS check | Security awareness |
| **Page load speed** | Performance metrics | Technical priority |
| **Accessibility** | Basic a11y checks | Compliance awareness |

### Analysis Output

```yaml
website_analysis:
  business_id: "biz_sunrise_dental"
  url: "https://sunrisedental.com"
  analyzed_at: "2026-07-04T12:05:00Z"
  
  technology:
    cms: "WordPress"
    hosting: "GoDaddy"
    ssl: true
    mobile_responsive: true
    page_load_time: "3.2s"
  
  gaps_identified:
    critical:
      - id: "no_chatbot"
        category: "customer_communication"
        description: "No chatbot or live chat detected on the website"
        opportunity: "Deploy AI chatbot for patient inquiries, appointment booking"
        estimated_impact: "high"
      
      - id: "manual_booking"
        category: "operational"
        description: "Patients must call to schedule appointments"
        opportunity: "AI scheduling assistant with calendar integration"
        estimated_impact: "high"
    
    moderate:
      - id: "basic_contact_form"
        category: "lead_capture"
        description: "Simple contact form with no qualification or instant response"
        opportunity: "Smart intake form with AI-powered instant response"
        estimated_impact: "medium"
    
    minor:
      - id: "no_faq"
        category: "operational"
        description: "No FAQ section for common patient questions"
        opportunity: "AI-powered FAQ and knowledge base"
        estimated_impact: "low"
  
  gap_count:
    critical: 2
    moderate: 1
    minor: 1
    total: 4
  
  overall_automation_readiness: "high"
  recommended_services:
    - "AI Chatbot (Professional tier)"
    - "AI Voice Agent (Basic tier)"
    - "Workflow Automation (Appointment scheduling)"
```

> **Full agent spec →** [Website Analysis Agent](../agents/website-analysis-agent.md)

---

## Component 3: Opportunity Scoring Agent

### Purpose
Score each analyzed business on a 0–100 scale based on how valuable they are as a potential Kaevo AI client.

### Scoring Model

#### Dimension Weights

| Dimension | Weight | Description |
|-----------|--------|-------------|
| **Automation Gap Severity** | 30% | How many and how critical are the gaps? |
| **Industry Fit** | 20% | Is this industry in Kaevo AI's sweet spot? |
| **Company Size & Revenue** | 15% | Can they afford our services? |
| **Technology Readiness** | 15% | How easy will integration be? |
| **Engagement Signals** | 10% | Online activity, review responsiveness, growth indicators |
| **Competition Exposure** | 10% | Are competitors already using AI automation? |

#### Dimension 1: Automation Gap Severity (30%)

| Criteria | Points (0–100) |
|----------|----------------|
| 4+ critical gaps | 90–100 |
| 2–3 critical gaps | 70–89 |
| 1 critical gap + moderate gaps | 50–69 |
| Only moderate/minor gaps | 30–49 |
| Few minor gaps only | 10–29 |
| No significant gaps | 0–9 |

#### Dimension 2: Industry Fit (20%)

| Tier | Industries | Points |
|------|-----------|--------|
| Tier 1 (Ideal) | Healthcare, Dental, Legal, Real Estate, SaaS, E-commerce | 80–100 |
| Tier 2 (Good) | Financial Services, Education, Hospitality, Professional Services | 60–79 |
| Tier 3 (Moderate) | Manufacturing, Retail, Construction, Non-profit | 40–59 |
| Tier 4 (Low) | Government, Agriculture, Heavy Industry | 10–39 |

#### Dimension 3: Company Size & Revenue (15%)

| Size | Estimated Revenue | Points |
|------|------------------|--------|
| 50–200 employees | $5M–$50M | 80–100 |
| 20–50 employees | $1M–$5M | 60–79 |
| 10–20 employees | $500K–$1M | 50–69 |
| 5–10 employees | $100K–$500K | 30–49 |
| 1–5 employees | < $100K | 10–29 |

#### Dimension 4: Technology Readiness (15%)

| Signal | Points |
|--------|--------|
| Modern tech stack (React, custom CMS) | 80–100 |
| Standard CMS (WordPress, Shopify) | 60–79 |
| Basic website builder (Wix, Squarespace) | 40–59 |
| Very basic or outdated site | 20–39 |
| No website | 0–19 |

#### Dimension 5: Engagement Signals (10%)

| Signal | Points |
|--------|--------|
| Active social media + responding to reviews + growing | 80–100 |
| Active on 2+ platforms | 60–79 |
| Some social presence | 40–59 |
| Minimal online activity | 20–39 |
| No engagement signals | 0–19 |

#### Dimension 6: Competition Exposure (10%)

| Signal | Points |
|--------|--------|
| Competitors visibly using AI/chatbots | 80–100 |
| Industry has high AI adoption | 60–79 |
| Some competitors modernizing | 40–59 |
| Industry slow to adopt | 20–39 |
| No competitive pressure | 0–19 |

### Composite Score Calculation

```
Total Score = (Gap Severity × 0.30)
            + (Industry Fit × 0.20)
            + (Company Size × 0.15)
            + (Tech Readiness × 0.15)
            + (Engagement × 0.10)
            + (Competition × 0.10)
```

### Score Thresholds

| Score Range | Classification | Action |
|-------------|---------------|--------|
| **80–100** | 🔥 Hot Opportunity | Priority outreach, personalized proposal, immediate follow-up |
| **60–79** | 🟢 Strong Opportunity | Standard outreach with personalized proposal |
| **40–59** | 🟡 Moderate Opportunity | Template outreach, nurture sequence |
| **20–39** | 🟠 Low Opportunity | Archive, re-score quarterly |
| **0–19** | ⚪ Not a Fit | Archive, exclude from outreach |

### Score Output

```yaml
opportunity_score:
  business_id: "biz_sunrise_dental"
  scored_at: "2026-07-04T12:10:00Z"
  
  dimensions:
    automation_gap_severity: { score: 85, weight: 0.30, weighted: 25.5 }
    industry_fit: { score: 90, weight: 0.20, weighted: 18.0 }
    company_size: { score: 60, weight: 0.15, weighted: 9.0 }
    technology_readiness: { score: 65, weight: 0.15, weighted: 9.75 }
    engagement_signals: { score: 70, weight: 0.10, weighted: 7.0 }
    competition_exposure: { score: 55, weight: 0.10, weighted: 5.5 }
  
  total_score: 74.75
  classification: "strong_opportunity"
  action: "standard_outreach_with_personalized_proposal"
  priority_rank: 12  # out of current batch
```

> **Full agent spec →** [Opportunity Scoring Agent](../agents/opportunity-scoring-agent.md)

---

## Component 4: Personalized Proposal Agent

### Purpose
Generate customized AI automation recommendations based on the website analysis findings, tailored to the specific business.

### Proposal Generation Logic

```
Analysis Findings          Mapped Solutions
─────────────────          ────────────────
No chatbot         ──────► AI Chatbot (tier based on size)
Manual booking     ──────► AI Scheduling Assistant
No lead capture    ──────► Smart Lead Capture System
Phone-only contact ──────► AI Voice Agent
No FAQ             ──────► AI Knowledge Base
Poor response time ──────► 24/7 AI Support Agent
Manual intake      ──────► AI Intake Automation
No client portal   ──────► Client Dashboard
```

### Proposal Structure (Outbound)

| Section | Content | Data Source |
|---------|---------|-------------|
| **Personalized Opening** | "We noticed [specific gap] on [website]" | Website Analysis |
| **Gap Summary** | "3 areas where AI could save you [hours/money]" | Analysis gaps |
| **Recommended Solutions** | Specific AI services mapped to gaps | Gap → Service mapping |
| **Expected Results** | "Similar businesses saved X hours/week" | Industry benchmarks |
| **Investment Overview** | High-level pricing range | Service tiers + scope |
| **Next Step** | "Book a free 15-min strategy call" | Booking link |

### Personalization Variables

| Variable | Source | Example |
|----------|--------|---------|
| `{business_name}` | Discovery | "Sunrise Dental Clinic" |
| `{industry}` | Discovery | "dental clinic" |
| `{city}` | Discovery | "Austin" |
| `{primary_gap}` | Analysis | "no chatbot on your website" |
| `{gap_count}` | Analysis | "3 automation opportunities" |
| `{competitor_mention}` | Scoring | "other dental practices in Austin" |
| `{estimated_savings}` | Benchmarks | "15 hours per week" |
| `{recommended_service}` | Mapping | "AI Patient Support Bot" |

### Output

The agent produces a structured recommendation document stored in the CRM and used by the Outreach Email Agent to craft personalized outreach.

> **Full agent spec →** [Personalized Proposal Agent](../agents/personalized-proposal-agent.md)

---

## Component 5: Outreach Email Agent

### Purpose
Prepare personalized outreach emails for human review and compliant sending.

### Personalization Strategy

#### Tier 1 — Hot Opportunities (Score 80–100)

| Element | Strategy |
|---------|----------|
| Subject Line | Hyper-personalized: "{business_name} + AI = [specific outcome]" |
| Opening | Reference specific website gap observed |
| Body | Custom proposal summary with ROI projection |
| CTA | Direct booking link for strategy call |
| Follow-ups | 3-email sequence over 10 days |

#### Tier 2 — Strong Opportunities (Score 60–79)

| Element | Strategy |
|---------|----------|
| Subject Line | Industry-personalized: "How {industry} businesses save 20h/week" |
| Opening | Industry-specific pain point |
| Body | Relevant case study + gap summary |
| CTA | Reply for free analysis report |
| Follow-ups | 3-email sequence over 14 days |

#### Tier 3 — Moderate Opportunities (Score 40–59)

| Element | Strategy |
|---------|----------|
| Subject Line | Value-led: "Quick question about {business_name}" |
| Opening | Soft observation about their online presence |
| Body | Educational content + subtle pitch |
| CTA | Link to relevant blog post or free resource |
| Follow-ups | 2-email sequence over 14 days |

### Email Sequence Design

#### Sequence: Hot Opportunity (3 Emails)

**Email 1 — Day 0 (Initial Outreach)**
```
Subject: {business_name} — Found 3 ways AI could save you 15h/week

Hi {first_name},

I was looking at {website} and noticed a few areas where AI automation 
could make a real difference for {business_name}:

1. {primary_gap_description}
2. {secondary_gap_description}  
3. {tertiary_gap_description}

We recently helped a {industry} business in {similar_city} reduce their 
{pain_point} by {result_percentage}%.

Would a quick 15-minute call make sense to explore this?

[Book a Free Strategy Call →]

Best,
{sender_name}
Kaevo AI
```

**Email 2 — Day 4 (Value Follow-up)**
```
Subject: Re: {business_name} — Quick follow-up

Hi {first_name},

Following up on my note about {primary_gap}.

Here's a case study of how we helped {similar_business} in {industry}:
• Challenge: {challenge}
• Solution: {solution}  
• Result: {result}

Happy to share how something similar could work for {business_name}.

[See the Full Case Study →]

{sender_name}
```

**Email 3 — Day 10 (Final Touch)**
```
Subject: Last note — {business_name}

Hi {first_name},

Just wanted to send one final note. If AI automation isn't a priority 
right now, no worries at all.

If things change, here's a free resource that might be useful:
[How {industry} Businesses Are Using AI in 2026 →]

Wishing {business_name} the best.

{sender_name}
```

### Safety & Compliance

#### CAN-SPAM Compliance

| Requirement | Implementation |
|-------------|---------------|
| Accurate sender information | Verified Kaevo AI domain and identity |
| No deceptive subject lines | Subject must accurately reflect content |
| Identified as advertisement | Include business context (not disguised) |
| Physical address included | Kaevo AI business address in footer |
| Opt-out mechanism | Unsubscribe link in every email |
| Opt-out honored within 10 days | Automated suppression list management |

#### GDPR Compliance (EU Contacts)

| Requirement | Implementation |
|-------------|---------------|
| Lawful basis | Legitimate interest (B2B outreach with public data) |
| Data minimization | Only collect publicly available information |
| Right to erasure | Honor deletion requests within 30 days |
| Transparency | Clear explanation of why they're being contacted |
| Data processing records | Full audit trail of data collection and use |

#### Safety Guardrails

| Guardrail | Implementation |
|-----------|---------------|
| Human review | All emails queued for human approval before sending |
| Send rate limiting | Maximum 50 emails/day per sending account |
| Bounce monitoring | Auto-pause if bounce rate exceeds 5% |
| Spam score check | Pre-send spam score evaluation |
| Suppression list | Global do-not-contact list respected |
| Domain warm-up | Gradual increase in send volume for new domains |
| Reply detection | Auto-pause sequence on reply (positive or negative) |
| Unsubscribe processing | Immediate removal on unsubscribe request |

### Email Infrastructure

| Component | Technology |
|-----------|-----------|
| Sending | Resend or SendGrid (transactional) |
| Tracking | Open tracking (pixel), click tracking (redirect) |
| Templates | Stored in database, version-controlled |
| Queue | Redis-backed job queue with scheduled sends |
| Analytics | Open rate, click rate, reply rate, bounce rate |

> **Full agent spec →** [Outreach Email Agent](../agents/outreach-email-agent.md)

---

## Component 6: CRM Integration

### Purpose
Unified data store for the entire outbound pipeline, integrated with System 1's CRM.

### Data Model

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   Business   │────▶│   Contact    │────▶│  Interaction  │
│              │     │              │     │               │
│ name         │     │ first_name   │     │ type          │
│ website      │     │ last_name    │     │ channel       │
│ industry     │     │ email        │     │ content       │
│ location     │     │ phone        │     │ timestamp     │
│ size         │     │ role         │     │ agent_id      │
│ source       │     │ linkedin     │     │ outcome       │
└──────┬───────┘     └──────────────┘     └───────────────┘
       │
       ▼
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   Analysis   │     │  Outreach    │     │   Meeting    │
│              │     │   History    │     │              │
│ gaps[]       │     │              │     │ datetime     │
│ tech_stack   │     │ email_id     │     │ type         │
│ score        │     │ sequence_no  │     │ status       │
│ opportunities│     │ status       │     │ notes        │
│ analyzed_at  │     │ opened_at    │     │ recording    │
└──────────────┘     │ clicked_at   │     │ outcome      │
                     │ replied_at   │     └──────────────┘
                     └──────────────┘
```

### Pipeline Stages (Outbound)

```
Discovered → Analyzed → Scored → Proposal Generated → Email Sent →
Opened → Clicked → Replied → Meeting Booked → Won / Lost / Nurture
```

### Status Tracking

| Status | Description | Next Action |
|--------|-------------|-------------|
| `discovered` | Business found, pending analysis | Queue for Website Analysis Agent |
| `analyzed` | Website analyzed, gaps identified | Queue for Opportunity Scoring |
| `scored` | Opportunity score calculated | Route by score threshold |
| `proposal_generated` | Custom proposal created | Queue for email drafting |
| `email_drafted` | Outreach email drafted | Queue for human review |
| `email_approved` | Human approved, ready to send | Schedule for sending |
| `email_sent` | Email delivered | Monitor for engagement |
| `email_opened` | Recipient opened email | Wait for further action |
| `email_clicked` | Recipient clicked a link | Notify sales team |
| `replied_positive` | Positive reply received | Route to System 1 (booking) |
| `replied_negative` | Negative reply / unsubscribe | Suppress, add to do-not-contact |
| `meeting_booked` | Strategy call scheduled | Prepare pre-meeting brief |
| `converted` | Became a paying client | Transfer to Client pipeline |
| `lost` | Not interested or disqualified | Archive, re-score in 90 days |
| `nurture` | Interested but not ready | Add to nurture email sequence |

---

## Component 7: Analytics Dashboard

### Purpose
Real-time visibility into the entire outbound growth engine performance.

### Dashboard Sections

#### 1. Pipeline Overview

| Metric | Visualization |
|--------|--------------|
| Total businesses discovered | Counter |
| Currently in pipeline | Counter with stage breakdown |
| This week's discoveries | Trend line |
| Pipeline flow | Funnel chart (Discovery → Conversion) |

#### 2. Discovery Metrics

| Metric | Description |
|--------|-------------|
| Businesses discovered today/week/month | Volume tracking |
| By industry breakdown | Pie chart |
| By location breakdown | Map visualization |
| Discovery source distribution | Bar chart |
| Data quality score | Average completeness |

#### 3. Analysis Metrics

| Metric | Description |
|--------|-------------|
| Websites analyzed today/week/month | Volume tracking |
| Average gaps per business | Trend line |
| Most common gaps | Ranked bar chart |
| Analysis completion rate | Percentage |
| Average analysis time | Latency metric |

#### 4. Scoring Metrics

| Metric | Description |
|--------|-------------|
| Score distribution | Histogram |
| Average score by industry | Grouped bar chart |
| Hot opportunities this week | Counter + list |
| Score accuracy (vs. actual conversion) | Calibration metric |

#### 5. Outreach Metrics

| Metric | Description |
|--------|-------------|
| Emails drafted / approved / sent | Funnel counts |
| Open rate | Percentage (target: > 40%) |
| Click rate | Percentage (target: > 5%) |
| Reply rate | Percentage (target: > 3%) |
| Positive reply rate | Percentage |
| Bounce rate | Percentage (alert if > 5%) |
| Unsubscribe rate | Percentage (alert if > 1%) |

#### 6. Conversion Metrics

| Metric | Description |
|--------|-------------|
| Meetings booked from outbound | Counter + rate |
| Outbound → Client conversion rate | Percentage |
| Average deal size (outbound) | Dollar amount |
| Revenue from outbound pipeline | Total + trend |
| Cost per lead | Total spend / leads generated |
| Time to conversion | Average days from discovery to close |

#### 7. Agent Performance

| Metric | Description |
|--------|-------------|
| Tasks processed per agent | Counter per agent type |
| Average processing time | Latency per agent |
| Error rate per agent | Percentage |
| LLM token usage per agent | Cost tracking |
| Quality score per agent | Human rating of outputs |

---

## Technology Stack (System 2)

| Layer | Technology |
|-------|-----------|
| Agent Framework | LangChain (Python) + CrewAI for multi-agent orchestration |
| LLM | GPT-4o (analysis), GPT-4o-mini (classification, extraction) |
| Web Scraping | Playwright (headless browser) + BeautifulSoup + httpx |
| Tech Detection | Custom Wappalyzer-style fingerprinting |
| Database | PostgreSQL (shared with System 1 CRM) |
| Queue | Redis + BullMQ or Celery |
| Email Sending | Resend or SendGrid |
| Email Tracking | Custom pixel + redirect tracking |
| Dashboard | Next.js + Recharts or Tremor |
| Scheduling | Cron-based scheduler for batch processing |
| Monitoring | Sentry + custom agent metrics |

---

## Batch Processing Architecture

System 2 runs as a **batch pipeline**, not real-time:

```
┌─────────────────────────────────────────────────┐
│                DAILY BATCH CYCLE                 │
├─────────────────────────────────────────────────┤
│                                                  │
│  06:00  Discovery Agent runs                     │
│         → Find 50–200 new businesses             │
│                                                  │
│  08:00  Analysis Agent processes queue            │
│         → Analyze websites from discovery         │
│                                                  │
│  10:00  Scoring Agent processes queue             │
│         → Score all analyzed businesses            │
│                                                  │
│  11:00  Proposal Agent processes queue            │
│         → Generate proposals for score ≥ 60       │
│                                                  │
│  12:00  Email Agent drafts outreach               │
│         → Create email sequences for proposals    │
│                                                  │
│  13:00  Human Review Window                       │
│         → Team reviews and approves emails        │
│                                                  │
│  14:00  Approved emails sent                      │
│         → Scheduled delivery with rate limiting   │
│                                                  │
│  18:00  Analytics refresh                         │
│         → Update dashboard with day's metrics     │
│                                                  │
└─────────────────────────────────────────────────┘
```

---

## Cross-References

| Document | Relevance |
|----------|-----------|
| [System Overview](system-overview.md) | How System 2 connects to System 1 |
| [System 1 — Agency Platform](system-1-agency-platform.md) | Where converted leads go |
| [Agent Registry](../agents/agent-registry.md) | All agent specifications |
| [Data Models](data-models.md) | Database schemas |
| [Security & Compliance](security-compliance.md) | Email compliance, data privacy |

---

> _"System 2 is the growth engine. It finds the clients before they find us."_
