# Kaevo AI — Data Flow Architecture

> Maps the lifecycle of data from the User Interface (UI), through the API and AI systems, and back to the UI.

---

## 1. General Data Flow Pattern (CQRS-lite)

To maintain a predictable and scalable frontend, Kaevo AI separates how data is fetched (Queries) from how it is modified (Commands/Mutations).

### Standard Query Flow (Read)
1. **Component Mounts:** Dashboard or Page loads.
2. **React Query Hook:** `useQuery` fires, checking the local cache.
3. **Cache Miss/Stale:** Hook calls the `services/` API layer.
4. **API Request:** Next.js Serverless Route or FastAPI backend fetches from Supabase.
5. **UI Update:** React Query receives data, updates cache, and triggers a re-render.
6. **Background Sync:** Component remains subscribed to `stale-while-revalidate` background updates.

### Standard Mutation Flow (Write)
1. **User Action:** User clicks "Approve Proposal" (Agent B4 queue).
2. **React Query Mutation:** `useMutation` fires.
3. **Optimistic Update (Optional):** UI updates instantly to show the proposal as approved.
4. **API Request:** POST/PUT request sent to backend.
5. **Success:** Cache for the affected queries (e.g., `['proposals', 'pending']`) is invalidated, causing a fresh background fetch.
6. **Error:** Optimistic update rolls back, toast notification alerts the user.

---

## 2. AI Chat Message Lifecycle (Agent A1/A2)

The chat interaction requires a highly specialized, low-latency data flow utilizing Server-Sent Events (SSE).

### 1. The Request
- User types message and hits send.
- UI immediately appends a "User" message bubble to the local chat array.
- UI appends an empty "AI" message bubble with a `<StreamingIndicator />` (pulsing dots).
- Frontend sends a POST request to `/api/chat/message` with the `sessionId` and the new text.

### 2. The AI Processing (Backend)
- API routes the request to the **LLM Gateway**.
- Agent A1 (Receptionist) retrieves memory context and queries the Vector Store (RAG).
- LLM begins generating a response.

### 3. The Stream (SSE)
- Backend establishes an SSE connection with the client.
- As the LLM generates tokens, the backend emits them to the frontend in chunks.

### 4. The UI Render
- A custom hook (`useChatStream`) listens to the SSE stream.
- For every chunk received, it concatenates the text to the *last* message in the chat store.
- The React component rendering the active message bubble re-renders continuously (debounced if necessary for performance) to simulate human typing.

### 5. Finalization & Actions
- When the stream closes, the backend sends a final payload containing structured metadata (e.g., `intent: 'book_meeting'`).
- The frontend parses this metadata and conditionally renders rich interactive cards (like the Meeting Scheduler) inside the chat flow.

---

## 3. Proposal Generation Flow (Agent A3 / B4)

Generating a proposal is a long-running, asynchronous task.

### 1. Initiation
- Admin clicks "Generate Proposal" or Agent A2 auto-triggers it upon qualification.
- Frontend fires a mutation to `/api/proposals/generate`.

### 2. Acknowledgment
- Generating a complex proposal takes 30-60 seconds. We do NOT keep the HTTP connection open.
- Backend responds immediately with `202 Accepted` and a `jobId`.
- UI updates to show a "Generating..." state (progress bar or animated graphic).

### 3. Polling / WebSockets
- Frontend polls a status endpoint `/api/jobs/{jobId}` every 3 seconds, or listens via WebSocket.
- Backend processes the task via Queue Workers (Redis).

### 4. Completion
- Polling returns `status: 'completed'` along with the `proposalId`.
- UI invalidates the proposal cache.
- The generated `<InteractiveProposalCard />` replaces the loading state, allowing the admin to review, edit, or approve the document.

---

## 4. Dashboard Data Updates (Telemetry)

For the Client Dashboard, showing real-time or near-real-time metrics (Telemetry) proves the value of the Kaevo AI service.

- **Data Source:** Aggregate analytics queries from the central database.
- **Polling Strategy:** For high-priority metrics (e.g., "Active AI Conversations"), the frontend uses React Query's `refetchInterval` to poll every 15-30 seconds while the tab is active.
- **Window Focus Refetch:** Using React Query, data automatically refreshes whenever the client switches back to the Kaevo AI tab, ensuring they always see the most up-to-date metrics without manual reloading.
