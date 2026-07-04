# Kaevo AI — State Management Strategy

> Defines how data is stored, updated, and accessed across the Kaevo AI frontend.

---

## 1. The Tri-State Architecture

Modern React applications suffer when all state is forced into a single global store (like legacy Redux). Kaevo AI categorizes state into three distinct layers:

### A. Server State (React Query / SWR)
- **Definition:** Data that lives on the server and is cached on the client.
- **Tooling:** TanStack Query (React Query).
- **Use Cases:** 
  - CRM Pipeline data (Leads, Deals).
  - Client project telemetry.
  - Agent approval queues.
- **Strategy:** 
  - Treat the server as the source of truth.
  - Use aggressive caching and background refetching (stale-while-revalidate).
  - Use optimistic UI updates for pipeline drag-and-drop (update the UI instantly, rollback if the API fails).

### B. Global Client State (Zustand)
- **Definition:** UI state that must be accessed by disparate components across the app tree.
- **Tooling:** Zustand (Lightweight, unopinionated, avoids React Context re-render hell).
- **Use Cases:**
  - **User Session:** `useAuthStore` (Current user ID, role, permissions).
  - **Global Chat Widget:** `useChatWidgetStore` (Is the widget open/closed? What is the current minimize state?).
  - **Theme:** `useThemeStore` (Dark/Light mode override).
- **Strategy:** 
  - Keep it minimal. Do NOT store API data here.

### C. Local Component State (React `useState` / `useReducer`)
- **Definition:** Ephemeral state that only matters to a specific component or its immediate children.
- **Tooling:** React's built-in hooks.
- **Use Cases:**
  - Form inputs before submission.
  - Accordion open/close state.
  - Hover states or minor UI toggles.

---

## 2. AI Chat State Handling (Crucial)

The AI chat is the most complex state interaction in the app due to streaming responses and memory.

### The Chat Store (`features/ai-chat/store`)
Manages the active conversation session.
- **State Array:** `messages: [{ id, role: 'user'|'assistant', content, timestamp, status }]`
- **Streaming State:** `isTyping: boolean`, `currentStreamId: string | null`
- **Context:** `sessionId: string` (to link to the backend memory manager).

### Streaming Data Flow
1. User sends message -> Local state updates immediately (optimistic).
2. Store sets `isTyping = true`.
3. Frontend establishes a Server-Sent Events (SSE) or WebSocket connection.
4. As chunks arrive, the store updates the *last* message in the array incrementally.
5. React deeply optimizes renders to only update the active message bubble, not the whole list.

---

## 3. Form State & Validation

Forms (Qualification, Auth, Settings) require robust state management to handle complex validations.

- **Tooling:** React Hook Form + Zod.
- **Why:** React Hook Form uses uncontrolled components to prevent massive re-renders on every keystroke. Zod provides strict TypeScript schema validation that shares types with the backend API.
- **Flow:**
  1. Define Zod schema (e.g., `EmailSchema`).
  2. Bind to React Hook Form.
  3. Form state (isDirty, isValid, errors) is handled entirely within the local feature component.

---

## 4. Scalability for SaaS (Phase 5)

As Kaevo AI scales to a multi-tenant SaaS platform:

- **Tenant Isolation:** The Global `useAuthStore` will expand to include `currentTenantId`.
- **Query Keys:** All Server State (React Query) keys will strictly include the `tenantId` to prevent cache bleed between accounts.
  - *Bad:* `useQuery(['leads'])`
  - *Good:* `useQuery(['leads', tenantId])`
- **URL as State:** For dashboards, prioritize using the URL query parameters for state (e.g., `?status=qualified&sort=desc`). This ensures deep-linking works and users can share specific dashboard views with colleagues.
