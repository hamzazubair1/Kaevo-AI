# Kaevo AI — Full System Integration & MVP Execution Plan

> The master integration blueprint for Kaevo AI. This document maps the connections between the Frontend, Backend, AI Orchestrator, and Database, establishing the exact execution scope for the Minimum Viable Product (Phase 1).

---

## 1. End-to-End System Architecture

The Kaevo AI architecture is a unified loop where user intent translates into AI action and data persistence.

### The Request Lifecycle
1. **User Interaction (UI):** User interacts with the Next.js frontend (e.g., types a message in the Chat Interface).
2. **Edge API Layer:** Request hits the Next.js API Routes, passing through JWT authentication and Upstash Redis rate limiting.
3. **Backend Processing:** Node.js backend parses the request and writes a raw interaction log to the database.
4. **AI Orchestrator (The Brain):** The MASTER Orchestrator evaluates the intent, hydrates the context from Memory, and selects the appropriate AI Agent (e.g., Agent A2 - Lead Qualification).
5. **Agent Execution:** The Agent executes the task via LangChain/LlamaIndex, utilizing tools if necessary (e.g., querying CRM).
6. **Response Generation:** The LLM generates the response and tool-invocation metadata.
7. **Real-Time Return:** The backend streams the response back to the UI via Server-Sent Events (SSE).
8. **Memory Update:** A background BullMQ worker extracts key facts from the interaction and updates the Vector/Graph memory stores.

---

## 2. Frontend ↔ Backend Integration

The boundary between the React UI and the Node backend is strictly typed and authenticated.

### Communication Structure
- **Synchronous Data:** tRPC (or typed REST via Axios) for standard CRUD operations (Dashboards, Settings). Ensures end-to-end type safety.
- **Asynchronous Data:** React Query for data fetching, caching, and optimistic UI updates (e.g., Pipeline drag-and-drop).
- **Authentication Flow:** Supabase Auth handles session management. The frontend passes a `Bearer JWT` to the backend, which verifies it before processing.

---

## 3. AI Agent Integration Layer

Bridging the Node backend and the Python/Node Orchestrator.

### Triggering Agents
The frontend never talks to the Orchestrator directly. 
- UI calls `/api/v1/chat/message`.
- The backend API verifies the JWT, checks token limits, and then invokes the Orchestrator's `handleMessage()` function, passing the `tenant_id` and the user's prompt.

### Multi-Agent Collaboration
If the Orchestrator triggers a sub-agent (e.g., Execution Agent asking the Web Search Agent for data), this happens entirely server-side. The frontend is oblivious to the multi-agent chatter. It only receives the *final synthesized response* from the Orchestrator.

---

## 4. Memory System Integration

Connecting the AI Brain to persistent storage.

### Read Flow (Context Injection)
Before the Orchestrator sends a prompt to the LLM, the `ContextBuilder` module fires an asynchronous query to Pinecone (Vector DB), filtering strictly by `tenant_id`. The returned semantic matches (e.g., "Client prefers short emails") are injected into the System Prompt.

### Write Flow (Background Update)
To prevent blocking the user's chat experience, memory is updated asynchronously. Once a chat session ends, a BullMQ job triggers the `MemoryManagerAgent`. This agent reads the transcript, extracts facts, embeds them, and writes to Pinecone and Supabase.

---

## 5. Real-Time Chat System (CORE FEATURE)

The AI chat must feel as responsive as ChatGPT.

### Streaming Architecture
- **Protocol:** Server-Sent Events (SSE). Far lighter than WebSockets for unidirectional streams (LLM → Client).
- **Frontend Handling:** A custom React hook `useChatStream` listens to the SSE stream. It updates a local state variable `currentMessageChunk` on every tick.
- **Partial Rendering:** As chunks arrive, `react-markdown` continuously re-renders the active message bubble, creating the typewriter effect.
- **Tool Execution Updates:** If the AI is calling a tool, the backend sends a specific SSE event: `{"event": "tool_start", "name": "search_crm"}`. The frontend renders a subtle "Searching CRM..." UI indicator.

---

## 6. Database + Backend Sync Flow

Data persistence guarantees absolute traceability.

### Persistence Strategy
- **PostgreSQL (Supabase):** Stores deterministic data (Users, Sessions, Tenants, Lead Status).
- **Interaction Storage:** Every user prompt and full AI response is saved in a `chat_messages` table with a foreign key to `chat_sessions`. 
- **Logging (WORM):** Every tool invocation and LLM network request is logged to an immutable S3/R2 bucket for security auditing.

---

## 7. MVP Feature Scope

To launch efficiently, we must strictly bound the Phase 1 MVP.

### Included in MVP (Phase 1)
1. **Frontend:** Public marketing site, standalone AI Chat Interface (`/chat`), and a basic Admin Dashboard (Pipeline view).
2. **AI Agents:** System 1 Agents only (A1 Receptionist, A2 Qualification).
3. **Orchestrator:** Simple intent routing (no complex multi-agent swarms yet).
4. **Memory:** Short-term session memory only (Redis). (Vector DB deferred to V2).
5. **Database:** Supabase Auth and PostgreSQL CRM.
6. **Integration:** Working SSE chat streaming and basic email notifications.

### Excluded from MVP (Deferred to Phase 2+)
- System 2 (Outbound Email Generation).
- Complex Graph Memory.
- Self-serve Client SaaS registration (Kaevo operates as an agency for Phase 1).

---

## 8. System Bottlenecks & Optimization

Identifying risks in the MVP architecture.

### Bottlenecks
- **LLM Latency (Time to First Token - TTFT):** API calls to GPT-4o/Claude can lag. 
  - *Optimization:* The Orchestrator must immediately yield a "thinking" SSE event so the UI instantly reacts while waiting for the LLM.
- **Queue Backlog:** If traffic spikes, BullMQ workers might fall behind processing memory summaries.
  - *Optimization:* Kubernetes Horizontal Pod Autoscaler (HPA) configured to spin up new worker pods if queue depth > 100.

---

## 9. Deployment Architecture

The MVP will utilize a modern Serverless/Edge stack for rapid iteration.

### Hosting Strategy
- **Frontend & Edge API:** Deployed on **Vercel**. Provides global Edge caching, seamless Next.js App Router support, and zero-config CI/CD.
- **Database & Auth:** **Supabase** (Managed PostgreSQL, GoTrue Auth, and Row Level Security).
- **Cache & Rate Limiting:** **Upstash** (Serverless Redis) for extreme low-latency rate limiting and short-term chat memory.
- **Background Workers:** Render or Railway (for long-running Node.js BullMQ workers that cannot run on Vercel's serverless timeout limits).
- **Scaling Path:** As Kaevo AI hits enterprise scale (Phase 4), the backend and workers will migrate from Vercel/Railway to a dedicated AWS EKS (Kubernetes) cluster for fine-grained orchestration control.
