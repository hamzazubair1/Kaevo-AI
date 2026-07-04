# Kaevo AI — MVP Build Plan & Development Roadmap

> This document translates the Kaevo AI architectural designs into a concrete, executable engineering plan. It defines the exact tech stack, physical codebase structure, database schemas, and the day-by-day roadmap required to build the production-ready SaaS.

---

## 1. Executive Summary

We are transitioning from system design into **Code Execution**. 

This plan defines the development of **Kaevo AI Phase 1 (MVP)**. The goal of this phase is to build a functional, multi-tenant AI automation platform capable of handling authenticated users, streaming real-time AI conversations via the Orchestrator, and persisting data securely. 

We prioritize speed-to-market and robust scalability, relying heavily on modern serverless tooling and a strict Turborepo monorepo architecture.

---

## 2. Tech Stack Lock (Production)

| Layer | Technology | Justification |
|-------|------------|---------------|
| **Frontend Framework** | Next.js 14+ (App Router) | Best-in-class for SEO, SSR, and API Edge routing. |
| **UI & Styling** | Tailwind CSS + shadcn/ui | Rapid development matching our Design Tokens. |
| **State Management** | Zustand (Global) + React Query (Server) | Tri-state architecture; avoids Redux boilerplate. |
| **Backend Runtime** | Node.js (via Next.js API Edge) | Unified language (TypeScript) across full stack. |
| **AI Orchestration** | Vercel AI SDK + LangChain.js | Native SSE streaming support and robust tool execution. |
| **LLM Engine** | OpenAI (GPT-4o) / Anthropic (Claude 3.5) | Anthropic for fast routing, OpenAI for complex reasoning. |
| **Primary Database** | Supabase (PostgreSQL) | Out-of-the-box Auth, Row Level Security, and strict schema. |
| **Memory / Cache** | Upstash (Redis) + Pinecone (Vector) | Upstash for Chat Session memory; Pinecone for RAG. |
| **Hosting** | Vercel | Seamless deployment for Next.js and Edge functions. |

---

## 3. Codebase Structure (Turborepo)

To ensure the AI core logic remains cleanly separated from the frontend UI, we utilize a Monorepo.

```text
kaevo-ai/
├── apps/
│   ├── website/              # Next.js App: Public marketing & Client Dashboard
│   └── admin/                # Next.js App: Internal team dashboard
├── packages/
│   ├── ai-engine/            # The Brain: Orchestrator, Agents, and Tool logic
│   ├── database/             # Prisma or Drizzle schema and Supabase client
│   ├── ui/                   # Shared UI components (shadcn + Tailwind)
│   ├── config/               # ESLint, TypeScript base configs
│   └── memory-manager/       # Utilities for interacting with Redis & Pinecone
├── package.json
└── turbo.json
```

---

## 4. MVP Feature Breakdown

### Phase 1: The Core Brain (Current Focus)
- Secure User Authentication (Magic Links / OAuth via Supabase).
- Standalone AI Chat Interface with SSE streaming.
- **Agent A1 (Receptionist)** and **Agent A2 (Qualification)** fully operational.
- MASTER Orchestrator routing between Agent A1 and A2.
- Short-term session memory (Redis).

### Phase 2: The SaaS Platform (Next)
- Client Dashboard (Project tracking, Telemetry).
- Admin Dashboard (Pipeline CRM).
- Vector Memory integration (Pinecone) for deep RAG context.
- Additional Agents (Proposal Generator).

### Phase 3: Global Scale (Future)
- Self-serve multi-tenant registration.
- Stripe Billing integration.
- Advanced Agent Workflows (Outbound Email).

---

## 5. Development Roadmap (Day-by-Day Execution)

### Week 1: Foundation & Data Layer
- **Day 1:** Monorepo setup (Turborepo + Next.js apps). Lock ESLint/Prettier.
- **Day 2:** Supabase integration. Configure `packages/database`, deploy initial schema, set up Auth.
- **Day 3:** Build `packages/ui` (Atoms & Molecules) matching Day 3 Design Tokens.
- **Day 4:** Develop Public Marketing Site (Homepage, Services, About).
- **Day 5:** Implement Authentication UI (Login/Signup) and protect dashboard routes.

### Week 2: AI Engine & Orchestration
- **Day 6:** Build `packages/ai-engine`. Define the `BaseAgent` class and LLM API connections.
- **Day 7:** Implement the **MASTER Orchestrator** intent routing logic.
- **Day 8:** Build Agent A1 & A2 with specific system prompts and tool schemas.
- **Day 9:** Upstash Redis integration for Short-term Chat Memory.
- **Day 10:** Create the Streaming API Edge route (`/api/v1/chat`).

### Week 3: Integration & Launch
- **Day 11:** Frontend AI Chat UI (hooking up `useChatStream` and rendering markdown).
- **Day 12:** Implement UI partial rendering for AI Tool executions (Interactive Cards).
- **Day 13:** Basic Admin Dashboard UI to view intercepted leads.
- **Day 14:** E2E Testing, Vercel Production Deployment, and MVP Launch.

---

## 6. AI System Implementation Plan

- **Orchestrator:** Coded as a pure TypeScript function that takes an array of messages, queries a lightweight, fast LLM (Claude 3.5 Haiku) to classify intent (e.g., "qualify_lead"), and dynamically imports the correct Agent class.
- **Agents:** Implemented using Vercel AI SDK's `generateText` or `streamText`. They are bound to a Zod-validated tool schema.
- **Tools:** Independent async functions wrapped in `tool()` declarations (e.g., `search_crm_tool`).

---

## 7. API Design (Concrete Endpoints)

All endpoints reside in the Next.js `app/api/` directory.

- `POST /api/v1/auth/callback` - Supabase OAuth redirect handler.
- `POST /api/v1/chat/stream` - The core AI execution endpoint. Requires `Authorization` header. Body: `{ sessionId, message }`. Returns `text/event-stream`.
- `GET /api/v1/chat/history?sessionId=123` - Fetches previous messages for a specific chat.
- `GET /api/v1/leads` - Admin endpoint to fetch qualified users from the database.

---

## 8. Frontend Implementation Plan

1. **Layouts First:** Build `<MarketingLayout>` and `<DashboardLayout>`.
2. **Global State:** Wire up Zustand for `useAuthStore` to handle the session token globally.
3. **Chat Feed:** Build the `<MessageFeed>` component to handle rapid re-renders efficiently during SSE streaming.
4. **Interactive Cards:** Build the mapping logic so that if the AI returns `{"tool": "book_meeting"}`, the frontend dynamically renders the `<MeetingScheduler>` component inside the chat flow.

---

## 9. Database Schema Design (Phase 1)

Implemented via Prisma or Supabase SQL.

**Table: `users` (Managed via Supabase Auth)**
- `id` (UUID, PK)
- `email` (String)
- `role` (Enum: ADMIN, CLIENT, USER)
- `tenant_id` (UUID, FK)

**Table: `tenants` (For SaaS isolation)**
- `id` (UUID, PK)
- `name` (String)
- `stripe_customer_id` (String)

**Table: `chat_sessions`**
- `id` (UUID, PK)
- `tenant_id` (UUID, FK)
- `user_id` (UUID, FK) - Nullable if guest lead
- `status` (Enum: ACTIVE, ARCHIVED)
- `created_at` (Timestamp)

**Table: `chat_messages` (Long-term audit storage)**
- `id` (UUID, PK)
- `session_id` (UUID, FK)
- `role` (Enum: USER, ASSISTANT, SYSTEM)
- `content` (Text)
- `tool_calls` (JSONB)
- `created_at` (Timestamp)

**Table: `leads` (CRM)**
- `id` (UUID, PK)
- `tenant_id` (UUID, FK)
- `company_name` (String)
- `qualification_score` (Integer)
- `status` (Enum: NEW, QUALIFIED, MEETING_BOOKED)

---

## 10. Deployment Strategy

### Environments
- **Local:** `localhost:3000` connected to local Supabase instance (Docker) to prevent trashing production data during prompt testing.
- **Preview (Dev):** Vercel auto-deploys a unique URL for every GitHub Pull Request. Connected to a Staging Database.
- **Production:** `app.kaevo.ai`. Deployed on `main` branch merges. Connected to Production Supabase.

### CI/CD Pipeline
- GitHub Actions triggers on PR creation:
  - Runs ESLint and Prettier.
  - Runs TypeScript compilation check (`tsc --noEmit`).
  - Runs Unit tests on the AI Orchestrator logic to ensure tool routing hasn't broken.
- Upon passing, Vercel initiates the build and deployment.
