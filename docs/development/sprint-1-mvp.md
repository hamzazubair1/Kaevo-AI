# Kaevo AI — Sprint 1 (MVP) Implementation Plan

> **Goal:** Build the FIRST WORKING MVP SYSTEM in 7 days. No over-engineering. No multi-agent routing. Just a functional, end-to-end AI SaaS application.

---

## 1. Executive Summary

This sprint strips away all advanced architecture (Vector DBs, Multi-Agent Swarms, Kafka) to focus entirely on proving the core loop: A user logs in, sends a message to an AI, the backend processes it, and the AI responds using basic database memory. 

We are operating like a lean startup: shipping a minimal, working version fast.

---

## 2. MVP Scope Definition

**Build ONLY the following:**
- **Frontend:** Login page, Basic Dashboard, Chat UI.
- **Backend:** Auth API, Chat API, Message storage API.
- **AI Layer:** A single, hardcoded Agent A1 (Receptionist/Assistant) orchestrator.
- **Database:** 3 tables (`users`, `sessions`, `messages`).

---

## 3. Step-by-Step Build Plan (7 Days)

- **Step 1: Project Setup (Day 1)**
  - Scaffold the repository structure.
  - Install dependencies (Next.js, Tailwind, Supabase Client, Vercel AI SDK).
- **Step 2: Database Setup (Day 1)**
  - Initialize Supabase project. Create the 3 MVP tables.
- **Step 3: Auth System (Day 2)**
  - Implement Supabase Auth (Email/Password or Magic Link).
  - Create `/login` page and protect dashboard routes.
- **Step 4: Backend Setup (Day 3)**
  - Create Next.js API route structure (`/api/auth`, `/api/chat`).
- **Step 5: Chat System API & DB (Day 4)**
  - Implement POST `/api/chat/send` to save user messages to the database.
  - Implement GET `/api/chat/history` to load previous session messages.
- **Step 6: AI Response Layer (Day 5)**
  - Wire Vercel AI SDK to OpenAI/Anthropic inside `/api/chat/send`.
  - Fetch previous messages from DB and inject them into the LLM prompt.
- **Step 7: Frontend Setup (Day 6)**
  - Build the Chat UI (Input box, Message bubbles).
- **Step 8: Frontend-Backend Integration (Day 6)**
  - Connect Chat UI to the backend endpoints. Ensure basic streaming works.
- **Step 9: Testing MVP (Day 7)**
  - Run end-to-end tests: Register -> Login -> Chat -> Refresh Page (verify history loads).

---

## 4. Code Architecture (Real Structure)

A simplified, flat structure optimized for speed.

```text
kaevo-ai/
├── /frontend          # React/Next.js UI components (pages, hooks, styles)
├── /backend           # Next.js API Routes (/app/api) and server actions
├── /ai-core           # The single AI Agent logic and prompt definitions
├── /database          # Supabase client initialization and type definitions
└── /config            # Environment variables and Tailwind configuration
```
*Responsibility:* `/frontend` handles presentation. `/backend` handles HTTP and DB writes. `/ai-core` handles the LLM generation.

---

## 5. Simple AI Orchestrator Design

For this sprint, the "Orchestrator" is just a standard function. There is no multi-agent routing.

- **Single Agent First:** Hardcoded to `Agent A1 (General Assistant)`.
- **Prompt Generation:** The backend pulls the last 10 messages from the database for the current `session_id`, appends a static System Prompt, and sends the array to the LLM.
- **Response Return:** The Vercel AI SDK uses `streamText` to pipe the response back to the frontend in real-time.
- **Memory Injection:** Memory is strictly just the conversation history pulled via standard SQL `SELECT * FROM messages WHERE session_id = X`.

---

## 6. Database Design (Minimal MVP)

Only 3 tables required.

### 1. `users`
- `id` (UUID, Primary Key, matches Supabase Auth ID)
- `email` (String, Unique)
- `created_at` (Timestamp)

### 2. `sessions`
- `id` (UUID, Primary Key)
- `user_id` (UUID, Foreign Key -> users.id)
- `title` (String, e.g., "Chat - Oct 24")
- `created_at` (Timestamp)

### 3. `messages`
- `id` (UUID, Primary Key)
- `session_id` (UUID, Foreign Key -> sessions.id)
- `role` (String: 'user', 'assistant', 'system')
- `content` (Text)
- `created_at` (Timestamp)

---

## 7. API Endpoints (Real Implementation)

All endpoints live in the Next.js `/api` directory.

- **`POST /api/auth/register`** (or handled directly via Supabase client)
- **`POST /api/auth/login`** (or handled directly via Supabase client)
- **`POST /api/chat/send`**
  - *Payload:* `{ session_id: "uuid", message: "Hello" }`
  - *Action:* Saves user message to DB -> Calls LLM -> Streams response -> Saves AI response to DB.
- **`GET /api/chat/history`**
  - *Query:* `?session_id=uuid`
  - *Returns:* Array of message objects to populate the UI on page load.

---

## 8. Frontend Flow

The user journey is strictly linear.

1. **Login:** User arrives at `/login`. Enters credentials.
2. **Dashboard:** Redirected to `/dashboard`. Clicks "Start New Chat" (creates a `sessions` record).
3. **Chat:** Redirected to `/chat/[session_id]`.
4. **Interaction:** User types a message -> hits `POST /api/chat/send` -> Backend triggers AI -> Response streams into the UI -> Both messages are saved in the database.
5. **Refresh:** User reloads the page. `GET /api/chat/history` fires, rebuilding the chat UI exactly as they left it.

---

## 9. Testing Plan

Manual E2E (End-to-End) validation.
1. Create a brand new account.
2. Verify the database creates a `users` row.
3. Start a chat. Ask a question.
4. Verify the AI responds.
5. Check the database to ensure 2 new rows exist in `messages` (1 user, 1 assistant).
6. Hard refresh the browser. Verify the chat remains visible.

---

## 10. MVP Success Criteria

The sprint is marked **SUCCESSFUL** when:
- [ ] A user can create an account and login.
- [ ] A user can send a text message to the AI.
- [ ] The AI responds contextually via the backend.
- [ ] Messages persist in the database and survive a page reload.
- [ ] There are zero crashes in this specific flow.
