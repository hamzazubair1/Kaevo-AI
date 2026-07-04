# Kaevo AI — Backend Architecture & Orchestration Implementation

> The production-grade backend blueprint powering the Kaevo AI Multi-Tenant SaaS platform and Real-Time AI Execution Engine.

---

## 1. Backend Architecture Overview

To balance the speed of early SaaS development with the immense scaling requirements of a global AI Operating System, Kaevo AI utilizes an **Event-Driven Modular Monolith** transitioning to **Microservices**.

### Core Backend Modules
- **API Gateway (Edge):** Next.js App Router (Serverless) handling HTTP/WebSocket ingress, rate limiting, and JWT validation.
- **AI Orchestrator Module:** The central brain. Evaluates intent, assigns tasks to agents, and manages the LangChain/LlamaIndex execution loops.
- **CRM/Core Module:** Standard SaaS CRUD operations (Tenants, Users, Pipelines, Billing).
- **Worker/Job Module:** Dedicated Node.js (or Python) long-running processes reading from the Task Queue for asynchronous heavy lifting (e.g., outbound email drafting).

### Service Boundaries
While code may live in a monorepo (`apps/backend`, `packages/ai-engine`), the AI logic is strictly isolated from standard business logic. The CRM module cannot bypass the Orchestrator to directly trigger an LLM prompt.

---

## 2. AI Orchestration Backend Engine

The Orchestration backend is where the logic defined in Day 5 physically executes.

### Execution Lifecycle
1. **Intake:** Request hits `POST /api/v1/orchestrator`.
2. **Sync vs Async Evaluation:** 
   - *Chat/Conversational:* Handled **Synchronously** via Server-Sent Events (SSE) stream.
   - *Deep Work (Proposals/Outbound):* Handled **Asynchronously**. The backend immediately returns a `jobId` and pushes the task to the Message Queue.
3. **Agent Invocation:** The backend loads the Agent Class (e.g., `AgentA2`), hydrates its prompt with the `tenant_id` context, and binds the authorized tools.
4. **Execution Loop:** A `while(!goal_achieved)` loop runs. The LLM decides to use a tool, the backend executes the tool (e.g., SQL query), feeds the result back to the LLM, and continues until completion.

---

## 3. API Layer Design

### Strategy: RESTful with tRPC/GraphQL considerations
- **Client/SaaS Data:** Strict REST API (`/api/v1/leads`, `/api/v1/tenants`). Allows external customers to build against our API in the future.
- **Internal Frontend-to-Backend:** tRPC for end-to-end type safety between Next.js frontend and Node backend.

### Authentication & Rate Limiting
- **Flow:** Stateless JWTs signed by Supabase Auth.
- **Rate Limiting:** Managed at the API Edge via Upstash (Redis). Strict limits applied globally per `tenant_id` (e.g., 100 requests/min), with secondary AI-specific limits (e.g., 10 AI operations/min) to prevent catastrophic LLM bill shock.

---

## 4. Database Architecture

Kaevo AI relies on a polyglot persistence model to handle structured data, unstructured text, and high-dimensional vectors.

### Primary Databases
- **Relational DB (Supabase/PostgreSQL):** The source of truth for Users, Tenants, RBAC, Billing, and CRM pipelines. Uses strict Row Level Security (RLS).
- **Vector DB (Pinecone):** Stores semantic embeddings of client documents, past chat transcripts, and knowledge bases (Workspace Memory).
- **Graph DB (Neo4j - Future Phase):** Maps complex organizational hierarchies and entity relationships (e.g., mapping a specific Lead to an exact Product feature preference).

### Logging & Storage
- **Agent Logs:** Stored in a NoSQL Document DB (e.g., MongoDB or Postgres JSONB columns) to handle the massively dynamic schema of LLM execution traces.
- **Blob Storage:** AWS S3 / Cloudflare R2 for raw user uploads (PDFs, images) before they are parsed by the Document Processing Agent.

---

## 5. Task Queue System

AI generation is slow. Robust asynchronous processing is non-negotiable.

### Queue Architecture (Redis + BullMQ)
For Phases 1-3, **BullMQ backed by Redis** is utilized. It is native to Node/TypeScript and handles 99% of async AI workload needs.
*(Note: As the system scales to 10,000+ organizations in Phase 5, this will migrate to Apache Kafka for durable, replayable event streaming).*

### Job Processing System
- **Producers:** The API Gateway creates a job (`type: 'generate_proposal', payload: {...}`).
- **Consumers:** A fleet of autoscaling Worker Pods pull jobs from Redis.
- **Failure Handling:** 
  - If a job fails due to an LLM hallucination or external API timeout, BullMQ utilizes **Exponential Backoff Retries** (e.g., retry in 10s, then 30s, then 2m).
  - After 3 failures, the job is moved to a Dead Letter Queue (DLQ) and an alert fires to the engineering team.

---

## 6. Real-Time AI Execution Layer

For the conversational UI to feel alive, standard HTTP polling is insufficient.

### Architecture
- **Server-Sent Events (SSE):** Used for unidirectional streams (LLM → Client). When a user asks a question, the backend opens an SSE connection and streams the generated text tokens down in real-time. This is highly scalable over serverless edge functions.
- **WebSockets (Socket.io/Pusher):** Used for bidirectional real-time dashboard updates (e.g., an Admin watching a lead move across the Kanban board as the CRM Agent works in the background).

---

## 7. Multi-Tenant SaaS Architecture

True multi-tenancy ensures data privacy and scalable billing.

### Tenant Isolation Strategy (Logical Isolation)
- All clients share the same infrastructure (Database, Queues, Workers).
- **Enforcement:** Every table in the PostgreSQL database includes a `tenant_id` column.
- **Security Barrier:** Row Level Security (RLS) is applied at the database engine level. Even if the Node backend is compromised or an LLM attempts prompt injection, the database physically rejects any query lacking the correct `tenant_id` context in the session variable.

### Resource Boundaries
- AI Token usage is tracked asynchronously and written to a billing ledger per `tenant_id`. Rate limiters automatically suspend agent execution if a tenant exhausts their monthly SaaS tier limits.

---

## 8. AI Memory Integration Backend

How the Day 5 Memory Architecture connects to the backend:

1. **Write Pipeline:** 
   - A background BullMQ job periodically sweeps short-term session logs (Redis).
   - The Memory Agent summarizes the log using an LLM.
   - The backend calls an embedding model (e.g., `text-embedding-3-small`), receives a vector array, and inserts it into Pinecone with metadata `{"tenant_id": "123"}`.
2. **Retrieval Pipeline:**
   - When the Orchestrator boots an agent, the `ContextBuilder` module intercepts the prompt.
   - It executes an ANN (Approximate Nearest Neighbor) search against Pinecone, fetching the top 5 most relevant memories, and injects them into the LLM system prompt before execution.

---

## 9. Security & Authentication

### Roles and Access (RBAC)
- **Levels:** System Admin (Kaevo Team), Tenant Admin (Client Owner), Tenant User (Client Employee), and **System Agent**.
- **Agent Execution Rules:** Agents assume a constrained RBAC role when executing. If Agent B5 (Outbound) tries to call the `delete_user_account` tool, the backend authorization middleware intercepts the tool-call request and denies it, returning a 403 to the LLM.

### Audit Logs (WORM)
- Every significant action (Logins, Billing changes) AND every LLM execution trace (Prompt sent + Tool called + Response received) is written to a Write-Once-Read-Many (WORM) log. This ensures absolute traceability if an AI makes a catastrophic business decision.

---

## 10. Scalability Architecture

Designing for 10,000+ users and thousands of concurrent AI agents.

### Horizontal Scaling Plan
- **API Edge:** Deployed globally on Vercel/Cloudflare. Scales infinitely to handle web traffic and route requests.
- **AI Workers:** Containerized Node/Python workers deployed on Kubernetes (EKS/GKE). Auto-scaling groups trigger scaling based on the length of the BullMQ Task Queue. If the queue hits 500 pending jobs, K8s spins up 20 more worker pods automatically.
- **Database:** Supabase/PostgreSQL is scaled vertically initially, then horizontally via Read Replicas for heavy analytical queries on the Client Dashboard.

### Failover System
- If OpenAI experiences an outage, the Orchestrator's `LLMGateway` module automatically catches the 500 error, swaps the model configuration to Anthropic Claude or a self-hosted Llama 3 instance, and replays the prompt. This ensures 99.99% uptime for the SaaS platform despite upstream AI volatility.
