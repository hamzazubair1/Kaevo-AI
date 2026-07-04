# Kaevo AI — API Integration Design

> Strategy for the frontend API service layer, ensuring robust communication, error handling, and type safety.

---

## 1. API Service Layer Structure

The frontend interacts with the backend strictly through a centralized, typed API service layer. Components NEVER make raw `fetch` or `axios` calls directly.

### The API Client (Axios)
We use an Axios instance configured with base URLs, timeouts, and interceptors (`src/lib/axios.ts`).

```typescript
// Conceptual structure of the API Client
const apiClient = axios.create({
  baseURL: process.env.NEXT_PUBLIC_API_URL,
  timeout: 10000,
  headers: {
    'Content-Type': 'application/json',
  },
});
```

### Domain-Specific Services
API calls are grouped by feature domain inside `src/features/[feature]/services/`.

- `features/crm/services/leads.ts` (getLeads, updateLeadStatus)
- `features/ai-chat/services/chat.ts` (sendMessage, getHistory)
- `features/outbound/services/campaigns.ts` (startDiscovery, approveEmail)

---

## 2. Request & Response Interceptors

Interceptors act as middleware for all outgoing requests and incoming responses.

### Request Interceptor (Auth & Headers)
- Automatically attaches the current user's JWT (from Supabase Auth) to the `Authorization` header of every request.
- In a multi-tenant environment, attaches the active `X-Tenant-ID` header.

### Response Interceptor (Global Error Handling)
- Catches global errors before they reach the component level.
- **401 Unauthorized:** Automatically attempts to refresh the JWT. If the refresh fails, forces a logout and redirects to `/login`.
- **403 Forbidden:** Routes to an "Access Denied" view or shows a toast if a specific action is blocked by RBAC.
- **500 Server Error:** Logs the error to Sentry and triggers a generic global error toast ("An unexpected error occurred").

---

## 3. Type Safety (End-to-End)

To prevent runtime errors, the frontend and backend must share the exact same data contracts.

- **Zod Schemas:** Define API response shapes using Zod.
- **Type Extraction:** TypeScript interfaces are inferred directly from the Zod schemas (`type User = z.infer<typeof UserSchema>`).
- **Validation on Fetch:** The API service layer validates incoming data against the Zod schema. If the backend changes a field name unexpectedly, the error is caught at the network boundary, not deep within a React component.

---

## 4. Error Handling Strategy

Errors are categorized and handled based on user impact.

### 1. Network / Generic Errors
- Handled globally by the Axios response interceptor.
- Displayed via a non-intrusive Toast notification (`<Toast variant="danger" />`).

### 2. Validation / User Errors (400)
- Handled locally by the component.
- The API returns specific field errors (e.g., `{ email: "Invalid format" }`).
- React Query passes the error to React Hook Form, which displays the error message directly under the offending input field.

### 3. Critical Component Failures (React Error Boundaries)
- If a feature completely crashes (e.g., the Pipeline Kanban board receives mangled data), it is caught by a React `<ErrorBoundary>`.
- Renders a localized fallback UI (e.g., "Failed to load pipeline. [Retry Button]") instead of crashing the entire dashboard.

---

## 5. Loading States & Fallbacks

Good UX requires managing user expectations during network latency.

### 1. Skeleton Screens (Initial Load)
- When data is `isLoading` (first fetch), render a `<Skeleton />` version of the component that mimics the shape of the incoming data. This reduces perceived load time and layout shift (CLS).

### 2. Spinners & Disabling (Mutations)
- When a user submits a form or clicks an action button, the button immediately enters an `isLoading` state (shows a spinner, disables clicks) to prevent duplicate submissions.

### 3. Background Refreshing
- When data is `isFetching` (background refresh of stale data), do NOT show a blocking loader. Keep the existing data visible to the user. Optionally, show a very subtle, small loading indicator in a corner (e.g., "Syncing...").

---

## 6. Retry & Resilience Logic

- **Idempotent Requests (GET):** React Query is configured to automatically retry failed queries 3 times with exponential backoff before displaying a permanent error state.
- **Mutations (POST/PUT/DELETE):** Never auto-retry, as this could result in duplicate actions (e.g., sending an email twice or double-charging a client). Force the user to explicitly click retry.
