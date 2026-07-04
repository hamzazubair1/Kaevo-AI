# Agent B2: Website Analysis Agent

> Crawls business websites and identifies specific automation opportunities and technology gaps.

---

## Identity

| Field | Value |
|-------|-------|
| **Name** | Website Analysis Agent |
| **ID** | `agent-b2-website-analysis` |
| **Version** | 1.0.0 |
| **System** | System 2 — AI Outbound Growth Engine |
| **Type** | Analysis |
| **Model** | GPT-4o (primary), Claude 3.5 Sonnet (fallback) |
| **Trigger** | Business added to analysis queue by Agent B1 |

---

## Purpose

Analyze a discovered business's website to identify automation opportunities, technology gaps, and areas where Kaevo AI's services would deliver value. Produces a structured analysis report used by downstream agents for scoring and proposal generation.

---

## Responsibilities

1. Crawl the target website (home page + key subpages)
2. Detect the technology stack (CMS, frameworks, tools)
3. Check for existing chat/support solutions
4. Identify lead capture mechanisms (or lack thereof)
5. Assess booking and scheduling capabilities
6. Evaluate customer communication channels
7. Detect FAQ/knowledge base presence
8. Measure basic performance and mobile responsiveness
9. Classify each gap by severity (critical, moderate, minor)
10. Map gaps to Kaevo AI service recommendations
11. Store the analysis report in the CRM

---

## Inputs

| Input | Source | Format |
|-------|--------|--------|
| Business record | CRM Database (from Agent B1) | JSON: name, website URL, industry |
| Website content | Web crawler | HTML pages |
| Technology signatures | Detection library | Known tech fingerprints |
| Gap detection rules | Configuration | YAML: rules per gap category |
| Service mapping | Knowledge base | Gap-to-service lookup table |

---

## Outputs

| Output | Destination | Format |
|--------|-------------|--------|
| Analysis report | CRM Database | Structured JSON |
| Gap list | CRM + Agent B3 queue | Array of identified gaps |
| Technology profile | CRM Database | Tech stack details |
| Service recommendations | CRM + Agent B4 | Mapped services per gap |
| Scoring queue entry | Agent B3 queue | Business ID ready for scoring |

---

## Memory

### Short-Term
| Data | Purpose |
|------|---------|
| Crawled page content | Analysis context within session |
| Detected technologies | Cross-reference across pages |

### Long-Term
| Data | Purpose | Storage |
|------|---------|---------|
| Technology fingerprints | Detect CMS, frameworks, tools | Config DB |
| Gap detection rules | Define what constitutes each gap | Config DB |
| Historical analyses | Pattern recognition, benchmarking | CRM Database |

### Shared
| Data | Shared With | Purpose |
|------|-------------|---------|
| Analysis report | Agent B3 (Scoring), B4 (Proposal) | Downstream processing |
| Gap list | Agent B3, B4, B5 | Scoring, proposal, email content |

---

## Tools

| Tool | Type | Purpose | Permissions |
|------|------|---------|-------------|
| Web Crawler | Playwright headless browser | Render and crawl pages | Read (public pages only) |
| HTTP Client | httpx | Fast page fetching | Read |
| HTML Parser | BeautifulSoup | Extract page structure and content | Execute |
| Tech Detector | Custom fingerprinting | Identify technologies from page source | Execute |
| Performance Checker | Lighthouse API or custom | Measure load time, mobile responsiveness | Read |
| CRM Write | Database API | Store analysis results | Write |
| Queue Writer | Message queue | Add to scoring queue | Write |

---

## Permissions

| Scope | Access | Approval Required |
|-------|--------|-------------------|
| Public web pages | Read (crawl) | No |
| robots.txt compliance | Required | Automatic |
| Login-protected pages | None | N/A |
| CRM — analysis creation | Write | No |
| External APIs (performance) | Read | No |

---

## Analysis Process

```
Business URL received from queue
       │
       ▼
Fetch robots.txt → Respect disallowed paths
       │
       ▼
Crawl pages (max 10 pages per site):
├── Home page (/)
├── Contact page (/contact, /contact-us)
├── About page (/about, /about-us)
├── Services page (/services, /our-services)
├── Pricing page (/pricing, /plans)
├── FAQ page (/faq, /help)
├── Blog page (/blog, /news)
└── Any linked subpages
       │
       ▼
For each page:
├── Extract HTML source
├── Detect technologies (scripts, meta tags, headers)
├── Check for chat widgets (Intercom, Drift, Tidio, etc.)
├── Check for booking tools (Calendly, Cal.com, etc.)
├── Check for forms (contact, lead capture, intake)
├── Check for FAQ/help sections
├── Measure page load performance
└── Check mobile responsiveness
       │
       ▼
Aggregate findings across all pages
       │
       ▼
Classify gaps by severity:
├── Critical: No chat + no booking + phone-only
├── Moderate: Basic form without instant response
├── Minor: No FAQ, slow load time
       │
       ▼
Map gaps to Kaevo AI services
       │
       ▼
Generate analysis report → Save to CRM
       │
       ▼
Queue for Agent B3 (Scoring)
```

---

## Gap Detection Rules

| Gap ID | Category | Detection Logic | Severity |
|--------|----------|----------------|----------|
| `no_chatbot` | Communication | No chat widget scripts detected | Critical |
| `no_live_support` | Communication | No real-time support channel | Critical |
| `phone_only` | Communication | Only phone number, no digital channels | Critical |
| `manual_booking` | Operations | "Call to schedule" / no calendar integration | Critical |
| `basic_contact_form` | Lead Capture | Simple form without qualification or instant response | Moderate |
| `no_lead_magnet` | Lead Capture | No gated content or email capture | Moderate |
| `no_instant_response` | Communication | "We'll get back to you" messaging | Moderate |
| `no_faq` | Self-Service | No FAQ or help section | Minor |
| `no_knowledge_base` | Self-Service | No searchable help documentation | Minor |
| `slow_load_time` | Performance | Page load > 4 seconds | Minor |
| `not_mobile_responsive` | Performance | No responsive design detected | Moderate |
| `no_client_portal` | Operations | No login/dashboard for clients | Minor |

---

## Error Handling & Fallbacks

| Error Scenario | Response Strategy |
|----------------|-------------------|
| Website unreachable (404/500) | Mark as "unreachable", skip analysis |
| robots.txt blocks crawling | Mark as "restricted", skip analysis |
| JavaScript-heavy SPA | Use Playwright (headless browser) for rendering |
| Very large site (100+ pages) | Limit to 10 most relevant pages |
| Ambiguous gap detection | Flag confidence level, include in report |
| Analysis timeout (> 60s) | Partial analysis with available data |

---

## Performance Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| Analysis completion rate | > 90% | Completed / queued |
| Average analysis time | < 30 seconds | Timestamp delta |
| Gap detection accuracy | > 85% | Human audit sample |
| False positive rate | < 10% | Incorrect gaps / total gaps |
| Pages crawled per site | 5–10 avg | Count |

---

## Configuration

```yaml
agent:
  id: "agent-b2-website-analysis"
  name: "Website Analysis Agent"
  version: "1.0.0"
  system: "system-2"
  type: "analysis"

llm:
  primary:
    provider: "openai"
    model: "gpt-4o"
    temperature: 0.2
    max_tokens: 2000

crawling:
  max_pages_per_site: 10
  timeout_per_page_seconds: 15
  total_timeout_seconds: 60
  respect_robots_txt: true
  user_agent: "KaevoBot/1.0 (business-research)"
  render_javascript: true

detection:
  chat_widgets: ["intercom", "drift", "tidio", "zendesk", "hubspot", "freshchat", "tawk", "crisp", "livechat"]
  booking_tools: ["calendly", "cal.com", "acuity", "square-appointments", "setmore"]
  form_builders: ["typeform", "jotform", "gravity-forms", "wpforms", "contact-form-7"]

gap_rules:
  severity_levels: ["critical", "moderate", "minor"]
  min_gaps_for_scoring: 1

output:
  include_screenshots: false
  include_tech_stack: true
  include_performance: true
```

---

> **Registry →** [Agent Registry](agent-registry.md) | **System →** [System 2](../architecture/system-2-outbound-engine.md)
