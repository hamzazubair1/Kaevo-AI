# Kaevo AI — Security & Compliance Architecture

> Security standards, data privacy, and regulatory compliance for Kaevo AI systems.

---

## 1. Compliance Frameworks

Kaevo AI operates globally and must adhere to strict regulatory frameworks, especially concerning automated outreach and data processing.

### Email Compliance (System 2 Outbound)
**CAN-SPAM Act (US) & General Best Practices**
- **Identification:** All outreach emails clearly identify the sender as a representative of Kaevo AI.
- **Physical Address:** Kaevo AI's valid physical postal address is included in all email footers.
- **Opt-Out:** Every automated outbound email includes a clear, functional unsubscribe link.
- **Honor Opt-Outs:** Opt-out requests are processed immediately (system requires < 10 days by law, we do it instantly via suppression lists).
- **No Deceptive Subjects:** Subject lines accurately reflect the content of the message.

**GDPR (Europe) / CCPA (California)**
- **Lawful Basis:** B2B outreach relies on "Legitimate Interest".
- **Data Minimization:** System 2 agents only scrape and store publicly available business data necessary for analysis and outreach.
- **Right to Erasure:** Client/Admin dashboards include functionality to permanently delete a business record and all associated data upon request.
- **Privacy Policy:** Transparently outlines AI processing and data storage (linked on website and in outreach).

---

## 2. System Security Architecture

### Authentication & Authorization
- **Identity Provider:** Supabase Auth handles user identity.
- **Protocols:** JWT (JSON Web Tokens) for API access, with short-lived access tokens and secure, HTTP-only refresh tokens.
- **RBAC (Role-Based Access Control):**
  - `Admin`: Full access to all dashboards, configuration, and approval queues.
  - `Sales`: Access to CRM, pipelines, and email review queues.
  - `Client`: Restricted access only to their specific project data in the Client Dashboard.
  - `Agent (Service Account)`: Scoped API keys with strict boundaries (e.g., Discovery Agent cannot read CRM deal amounts).

### Data Protection
- **Encryption in Transit:** TLS 1.3 enforced for all web traffic and API communications.
- **Encryption at Rest:** PostgreSQL database (Supabase) encrypted at the volume level (AES-256).
- **Secrets Management:** Environment variables managed securely via deployment platforms (Vercel/Railway). No secrets stored in source code.

---

## 3. AI & Agent Guardrails

AI agents present unique security challenges (e.g., prompt injection, hallucination).

### Agent Safeguards
1. **Human-in-the-Loop (HITL):** Critical actions, specifically **Email Sending (Agent B5)** and **Proposal Delivery (Agent A3)**, require explicit human review and approval in the Admin dashboard. The agents cannot dispatch communications externally on their own.
2. **Read-Only Scopes:** Agents like B1 (Discovery) and B2 (Analysis) are granted read-only access to the public web. They cannot execute transactions.
3. **Data Segregation:** The LLM Gateway ensures that prompt context is strictly isolated per tenant/client to prevent data leakage between client projects.
4. **Input Sanitization:** User input from the chat widget (Agent A1) is sanitized to mitigate prompt injection attempts before being passed to the LLM.

### Rate Limiting & Abuse Prevention
- **API Gateway:** Rate limiting (e.g., 100 requests/minute per IP) protects public endpoints (chat widget).
- **Outbound Throttling:** The Email Agent sending queue is throttled to mimic human sending behavior and protect domain reputation.
- **Bot Protection:** Cloudflare/Vercel Edge protection against DDoS and automated form submission spam.

---

## 4. Incident Response Plan

In the event of a suspected breach or AI malfunction:
1. **Kill Switch:** Admin dashboard includes a global "Pause Agents" toggle that immediately halts the Agent Task Queue (preventing any further discovery, scoring, or drafting).
2. **Revocation:** API keys and JWT secrets can be rotated instantly.
3. **Audit:** All agent actions and LLM calls are heavily logged. Logs are retained for 90 days to facilitate forensic review.
