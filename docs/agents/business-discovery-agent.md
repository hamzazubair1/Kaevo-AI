# Agent B1: Business Discovery Agent

> Finds target businesses across the internet based on industry, location, and keyword criteria.

---

## Identity

| Field | Value |
|-------|-------|
| **Name** | Business Discovery Agent |
| **ID** | `agent-b1-discovery` |
| **Version** | 1.0.0 |
| **System** | System 2 — AI Outbound Growth Engine |
| **Type** | Research |
| **Model** | GPT-4o-mini (primary), Gemini Flash (fallback) |
| **Trigger** | Scheduled batch (daily) or manual campaign launch |

---

## Purpose

Systematically discover publicly available businesses that match Kaevo AI's ideal customer profile. Sources include Google Maps, search engines, industry directories, and public business listings. Only uses publicly available data.

---

## Responsibilities

1. Execute search campaigns based on defined criteria (industry, location, keywords)
2. Extract business data from public sources (name, website, phone, location)
3. Deduplicate results against existing CRM records
4. Enrich basic profiles with publicly available information
5. Validate data quality (valid URLs, phone format, email format)
6. Queue validated businesses for website analysis
7. Track discovery volume and source quality metrics
8. Respect rate limits and terms of service for all data sources

---

## Inputs

| Input | Source | Format |
|-------|--------|--------|
| Search campaign config | Admin / scheduled job | YAML: industries, locations, keywords, limits |
| Existing CRM records | CRM Database | Business list for deduplication |
| Source API credentials | Environment variables | API keys for Google, directories |
| Suppression list | CRM Database | Do-not-contact businesses |

---

## Outputs

| Output | Destination | Format |
|--------|-------------|--------|
| Discovered business records | CRM Database | Structured JSON per business |
| Discovery batch report | Analytics Dashboard | Summary: count, sources, quality |
| Analysis queue entries | Agent B2 queue | Business IDs pending analysis |
| Error/skip log | Logging system | Skipped entries with reasons |

---

## Memory

### Short-Term
| Data | Purpose |
|------|---------|
| Current batch results | Deduplication within batch |
| API rate limit counters | Respect source limits |

### Long-Term
| Data | Purpose | Storage |
|------|---------|---------|
| All discovered businesses | Historical record, dedup | CRM Database |
| Source quality scores | Prioritize high-quality sources | Analytics DB |
| Search campaign history | Track what's been searched | Campaign DB |

### Shared
| Data | Shared With | Purpose |
|------|-------------|---------|
| Discovered business records | Agent B2 (Analysis) | Next pipeline step |
| Dedup data | All System 2 agents | Avoid reprocessing |

---

## Tools

| Tool | Type | Purpose | Permissions |
|------|------|---------|-------------|
| Google Maps API | External API | Location-based business search | Read |
| Google Custom Search | External API | Keyword-based web search | Read |
| Web Scraper | Playwright/httpx | Extract data from directories | Read |
| CRM Read | Database API | Deduplication check | Read |
| CRM Write | Database API | Save discovered businesses | Write |
| Data Validator | Internal function | Validate URLs, emails, phones | Execute |
| Queue Writer | Message queue | Add businesses to analysis queue | Write |

---

## Permissions

| Scope | Access | Approval Required |
|-------|--------|-------------------|
| Public web data | Read | No |
| Google Maps API | Read | No (within quota) |
| CRM — business creation | Write | No (automated) |
| CRM — contact creation | Write | No (public data only) |
| Private/paywalled data | None | N/A |
| Social media scraping | None | N/A (compliance risk) |

---

## Processing Logic

```
Campaign Configuration Loaded
       │
       ▼
For each (industry × location) pair:
       │
       ├── Search Google Maps API
       │   → Extract: name, address, phone, website, rating, category
       │
       ├── Search Google Custom Search
       │   → Extract: website, description, social links
       │
       └── Search Industry Directories
           → Extract: profile data, reviews, specializations
       │
       ▼
Merge results by business name + location
       │
       ▼
Deduplicate against CRM
       │
       ├── Already exists → Skip (or update if data is newer)
       └── New business → Continue
       │
       ▼
Validate data quality
       │
       ├── Valid website URL → Continue
       ├── Missing website → Flag, still save
       └── Invalid/spam data → Discard
       │
       ▼
Save to CRM with status: "discovered"
       │
       ▼
Add to Agent B2 analysis queue
```

---

## Error Handling & Fallbacks

| Error Scenario | Response Strategy |
|----------------|-------------------|
| API rate limit hit | Pause, resume after cooldown period |
| API key expired | Alert admin, pause campaign |
| No results for criteria | Log, suggest broader search terms |
| Duplicate detection uncertain | Flag for human review |
| Invalid URL format | Skip website, save other data |
| Source timeout | Retry 3x with backoff → skip source |

---

## Performance Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| Businesses discovered per batch | 50–200 | Count |
| Data completeness | > 80% | Fields filled / total fields |
| Deduplication accuracy | > 99% | False duplicates flagged / total |
| Discovery to analysis queue rate | > 90% | Queued / discovered |
| Average discovery cost per business | < $0.01 | API costs / businesses found |
| Source quality score | Track per source | Conversion rate from source |

---

## Configuration

```yaml
agent:
  id: "agent-b1-discovery"
  name: "Business Discovery Agent"
  version: "1.0.0"
  system: "system-2"
  type: "research"

llm:
  primary:
    provider: "openai"
    model: "gpt-4o-mini"
    temperature: 0.1
    max_tokens: 300

schedule:
  frequency: "daily"
  time: "06:00 UTC"
  batch_size: 100

sources:
  google_maps:
    enabled: true
    daily_quota: 500
  google_search:
    enabled: true
    daily_quota: 100
  directories:
    enabled: true
    sources: ["yelp", "clutch", "g2"]

targeting:
  industries:
    - "dental clinics"
    - "law firms"
    - "real estate agencies"
    - "e-commerce stores"
    - "SaaS companies"
  locations:
    - "United States"
    - "United Kingdom"
    - "Canada"
  keywords:
    - "contact us"
    - "book appointment"
    - "request quote"

quality:
  require_website: true
  min_data_fields: 3
  dedup_strategy: "name_location_fuzzy"
```

---

> **Registry →** [Agent Registry](agent-registry.md) | **System →** [System 2](../architecture/system-2-outbound-engine.md)
