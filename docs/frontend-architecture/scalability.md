# Kaevo AI — Scalability & SaaS Readiness

> Architecture design ensuring the frontend can evolve from an MVP Agency platform to a global Multi-Tenant SaaS (Phase 5).

---

## 1. Multi-Tenant Architecture Readiness

While Phase 1-2 focuses on Kaevo AI's internal team and clients, Phase 5 introduces a self-serve SaaS model where other agencies can use the platform.

### Tenant Identification
- **Subdomain Routing:** The frontend should support tenant identification via subdomains (e.g., `client-name.kaevo.ai`). Next.js Middleware intercepts the request, extracts the tenant slug from the hostname, and rewrites the URL to a dynamic route like `/app/[tenantId]/...`.
- **API Context:** The `tenantId` extracted from the URL is injected into the global `useAuthStore` and automatically attached to all API requests via the Axios interceptor (`X-Tenant-ID`).

### White-Labeling (Theme Overrides)
- The Design System (Day 3) is strictly built on CSS Variables (Tokens).
- To support white-labeling, the frontend simply injects a `<style>` block scoped to the `[data-tenant="xyz"]` attribute that overrides the root CSS variables (e.g., changing `color-brand-primary` from Electric Blue to a client's custom color).

---

## 2. Feature Toggles & Gradual Rollouts

To deploy new AI agents or features safely without disrupting existing users.

- **System:** Integration with a feature flag service (e.g., LaunchDarkly, PostHog, or a custom Supabase table).
- **Implementation:** 
  - Flags are fetched at session start and stored in a React Context or Zustand store (`useFeatureStore`).
  - Components use a custom hook: `const isV2ChatEnabled = useFeatureFlag('chat-v2');`.
  - This allows the team to roll out "System 2 Outbound Dashboard" to admins first, then specific beta clients, before a global launch.

---

## 3. Role-Based Access Control (RBAC) in UI

The UI must adapt based on the user's permissions to prevent unauthorized actions and reduce clutter.

### Route Protection
- Next.js Middleware checks the user's role (encoded in the JWT) before allowing access to `/admin/*` or `/client/*` routes.
- Unauthorized access attempts are instantly redirected to `/login` or a `403 Forbidden` page.

### Component-Level Protection
- A higher-order component (HOC) or wrapper is used to conditionally render UI elements based on permissions.
```tsx
<RequirePermission permission="proposals:approve">
  <Button onClick={approveProposal}>Approve to Send</Button>
</RequirePermission>
```
- If the user lacks the permission, the button is either hidden entirely or rendered in a disabled state with a tooltip ("Admin access required").

---

## 4. Internationalization (i18n) Preparation

To support a global SaaS platform in the future, the frontend architecture prevents hard-coded English strings.

### Strategy
- **Library:** Set up `next-intl` or `react-i18next` from the beginning.
- **Implementation:** All UI text (buttons, headers, placeholder text) is wrapped in a translation hook.
  - *Bad:* `<Button>Submit</Button>`
  - *Good:* `<Button>{t('common.submit')}</Button>`
- **AI Integration:** The language preference (detected from the browser or user settings) is passed to the LLM Gateway, ensuring the AI Receptionist responds in the user's preferred language.

---

## 5. Plugin-Based AI Agent Extensibility

As the Agent Marketplace (Phase 5) is introduced, the frontend must dynamically render controls for new, unknown agents.

- **Schema-Driven UI:** Instead of hardcoding settings forms for Agent A1, A2, etc., the frontend fetches a JSON Schema defining the agent's configuration options.
- **Dynamic Forms:** A component (e.g., `react-jsonschema-form`) automatically generates the settings UI (toggles, inputs, sliders) based on the schema. This allows new agents to be added to the platform without requiring a frontend redeploy.
