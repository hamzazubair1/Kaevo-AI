# Agent A1: AI Receptionist

> The front door of Kaevo AI. Every visitor's first AI interaction.

---

## Identity

| Field | Value |
|-------|-------|
| **Name** | AI Receptionist |
| **ID** | `agent-a1-receptionist` |
| **Version** | 1.0.0 |
| **System** | System 1 — AI Agency Platform |
| **Type** | Conversational |
| **Model** | GPT-4o (primary), Claude 3.5 Sonnet (fallback) |
| **Deployment** | Website chat widget, all pages |

---

## Purpose

The AI Receptionist is the first point of contact for every website visitor. It greets visitors, understands their intent, answers questions from the knowledge base, and routes them to the appropriate next step — qualification, booking, or self-service information.

---

## Responsibilities

1. Greet visitors with a warm, professional welcome message
2. Detect visitor intent from their initial message
3. Answer common questions using the Kaevo AI knowledge base (RAG)
4. Route qualified interest to the AI Qualification Assistant (Agent A2)
5. Enable direct meeting booking for visitors who are ready
6. Capture basic lead information (name, email, company) before handoff
7. Handle small talk and off-topic gracefully
8. Escalate to human support when confidence is low or requested
9. Maintain conversation context across the session
10. Log all interactions to the CRM

---

## Inputs

| Input | Source | Format |
|-------|--------|--------|
| Visitor message | Chat widget (WebSocket) | Text string |
| Page context | Current URL, page title | Metadata |
| Visitor info | Browser fingerprint, referral source | Metadata |
| Conversation history | Session memory | Message array |
| Knowledge base | Vector store (RAG) | Retrieved context chunks |
| Business hours | Configuration | Schedule object |
| Team availability | Calendar API | Boolean / next available slot |

---

## Outputs

| Output | Destination | Format |
|--------|-------------|--------|
| Response message | Chat widget → visitor | Text string |
| Intent classification | Internal routing | Enum: `qualify`, `book`, `info`, `support`, `other` |
| Lead data | CRM Database | JSON: name, email, company, intent |
| Handoff request | Agent A2 or booking system | Event with conversation context |
| Conversation log | CRM + Analytics | Structured interaction record |
| Escalation request | Human team notification | Alert with conversation transcript |

---

## Memory

### Short-Term (Session)
| Data | Purpose | Duration |
|------|---------|----------|
| Conversation messages | Maintain context within chat | Session lifetime |
| Detected intent | Track visitor's primary need | Session lifetime |
| Collected lead data | Accumulate info across messages | Session lifetime |
| Pages visited | Understand visitor behavior | Session lifetime |

### Long-Term (Persistent)
| Data | Purpose | Storage |
|------|---------|---------|
| Knowledge base | Answer questions about Kaevo AI | Vector store (Pinecone) |
| FAQ cache | Fast retrieval for common questions | Redis |
| Conversation patterns | Improve greeting and routing | Analytics DB |

### Shared (Cross-Agent)
| Data | Shared With | Purpose |
|------|-------------|---------|
| Lead profile (name, email, company, intent) | Agent A2, CRM | Seamless handoff without re-asking |
| Conversation transcript | Agent A2, Admin Dashboard | Full context for qualification |

---

## Tools

| Tool | Type | Purpose | Permissions |
|------|------|---------|-------------|
| Knowledge Base Search | Vector DB query | Answer questions from docs | Read |
| CRM Write | Database API | Save lead data and interactions | Write |
| Calendar Check | Cal.com API | Check team availability | Read |
| Handoff Trigger | Internal event | Transfer to Agent A2 or booking | Execute |
| Human Escalation | Notification API | Alert team for human takeover | Execute |
| Page Context Reader | Frontend API | Read current page visitor is on | Read |

---

## Permissions

| Scope | Access | Approval Required |
|-------|--------|-------------------|
| Knowledge base | Read | No |
| CRM — lead creation | Write | No (auto-logged) |
| CRM — existing lead update | Write | No |
| Calendar — availability check | Read | No |
| Calendar — booking creation | Execute | No (visitor-initiated) |
| Human escalation | Execute | No (auto on low confidence) |
| Visitor personal data | Read/Write | Consent collected via chat |
| Financial data | None | N/A |
| Admin settings | None | N/A |

---

## Conversation Flow

```
┌─────────────────────────────────────┐
│         Visitor Opens Chat          │
└────────────────┬────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────┐
│  Display greeting message           │
│  "Hi! I'm Kaevo, your AI          │
│   assistant. How can I help?"       │
└────────────────┬────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────┐
│  Receive visitor's first message    │
│  Classify intent:                   │
│  • QUALIFY — wants AI services      │
│  • BOOK — wants to schedule call    │
│  • INFO — wants to learn more       │
│  • SUPPORT — has existing issue     │
│  • OTHER — off-topic or unclear     │
└────────────────┬────────────────────┘
                 │
    ┌────────────┼────────────┬──────────────┐
    │            │            │              │
    ▼            ▼            ▼              ▼
 QUALIFY       BOOK         INFO          OTHER
    │            │            │              │
    ▼            ▼            ▼              ▼
 Collect      Check        Search          Clarify
 name +       calendar     knowledge       intent
 email        availability base            or
    │            │            │           escalate
    ▼            ▼            ▼              │
 Handoff      Show         Present          │
 to A2        slots        answer +         │
              + book       offer to         │
                           qualify          │
                              │             │
                              └─────────────┘
```

---

## Error Handling & Fallbacks

| Error Scenario | Response Strategy |
|----------------|-------------------|
| Intent unclear after 2 messages | Ask clarifying question with suggestions |
| Knowledge base miss (no relevant answer) | "I don't have that specific info. Let me connect you with our team." |
| LLM timeout or failure | "Give me a moment..." → Retry once → Fallback to human |
| Visitor frustrated or angry | Detect sentiment → Immediate human escalation |
| Visitor asks about competitors | Acknowledge → Redirect to Kaevo AI strengths |
| Spam or abuse detected | Politely disengage → Log → Block if repeated |
| Outside business hours | "Our team is offline, but I can help! [continue or leave message]" |

---

## Performance Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| First response time | < 1 second | Timestamp delta |
| Intent detection accuracy | > 90% | Human audit sample |
| Successful routing rate | > 85% | Correct handoff / total handoffs |
| Lead capture rate | > 60% | Leads captured / conversations started |
| Visitor satisfaction | > 4.0/5.0 | Post-chat rating |
| Escalation rate | < 15% | Human escalations / total conversations |
| Conversation completion rate | > 70% | Completed flows / started conversations |

---

## Configuration

```yaml
agent:
  id: "agent-a1-receptionist"
  name: "Kaevo AI Receptionist"
  version: "1.0.0"
  system: "system-1"
  type: "conversational"

llm:
  primary:
    provider: "openai"
    model: "gpt-4o"
    temperature: 0.4
    max_tokens: 500
  fallback:
    provider: "anthropic"
    model: "claude-3-5-sonnet"

knowledge:
  vector_store: "pinecone"
  index: "kaevo-knowledge-base"
  embedding_model: "text-embedding-3-small"
  top_k: 5
  similarity_threshold: 0.75

greeting:
  message: "Hi! 👋 I'm Kaevo, your AI assistant. How can I help you today?"
  delay_seconds: 5
  auto_open: true

routing:
  qualification_threshold: 0.7
  booking_direct: true
  human_escalation_confidence: 0.3

guardrails:
  max_conversation_turns: 30
  prohibited_topics: ["competitor_pricing", "internal_operations", "employee_info"]
  sentiment_escalation: true
  profanity_filter: true

logging:
  log_conversations: true
  log_to_crm: true
  analytics_events: true
```

---

> **Registry →** [Agent Registry](agent-registry.md) | **System →** [System 1](../architecture/system-1-agency-platform.md)
