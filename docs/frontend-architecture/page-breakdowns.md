# Kaevo AI — Page-to-Component Breakdown

> Deconstructs key pages into their constituent frontend components and defines the rendering hierarchy.

---

## 1. Public Website Pages

### Home Page (`/`)
- **LayoutWrapper:** `<MarketingLayout />`
- **Feature Components:**
  - `<HeroSection />`: Connects to routing (CTA buttons).
  - `<InteractiveValueDemo />`: Handles state for the Before/After slider.
  - `<GlobalChatWidget />` (floating): Connects to `features/ai-chat` store.
- **Shared Components:**
  - `<TrustBar />` (logos)
  - `<ServiceCardGrid />` (maps over data to render `<ServiceCard />` atoms)
  - `<TestimonialHighlight />`

### Services Page (`/services`)
- **LayoutWrapper:** `<MarketingLayout />`
- **Feature Components:**
  - `<PricingToggle />`: Manages state (Monthly/Annual).
- **Shared Components:**
  - `<PageHeader />`
  - `<FeatureAlternatingBlock />` (reusable left/right text-image split)
  - `<PricingCard />` (Organism)
  - `<TechStackGrid />`

### Contact / Book Meeting (`/book`)
- **LayoutWrapper:** `<FocusLayout />` (No header/footer to prevent funnel leakage)
- **Feature Components:**
  - `<MeetingScheduler />`: Wrapper around Cal.com API, handles booking success callbacks.
- **Shared Components:**
  - `<SplitScreenContainer />`
  - `<ValuePropositionList />`

---

## 2. Dedicated AI Interfaces

### AI Chat Interface (`/chat`)
- **LayoutWrapper:** `<FocusLayout />`
- **Feature Components (`features/ai-chat`):**
  - `<FullScreenChatContainer />`: Manages full-height scrolling and layout.
  - `<MessageFeed />`: Maps over chat history, auto-scrolls to bottom.
  - `<ChatInputArea />`: Handles typing state, form submission, and suggestion chip clicks.
  - `<StreamingIndicator />`: Renders when waiting for LLM chunks.
- **Shared Components:**
  - `<ChatMessage />` (User vs. AI variants)
  - `<SuggestionChip />`
  - `<InteractiveProposalCard />` (Rendered conditionally based on AI intent)

---

## 3. Dashboards

### Admin Dashboard (Internal)
- **LayoutWrapper:** `<DashboardLayout variant="admin" />`
- **Feature Components:**
  - `<PipelineBoard />`: Implements drag-and-drop context (e.g., `dnd-kit`), triggers API updates on drop.
  - `<LeadDetailDrawer />`: Connects to CRM store to fetch full lead context.
  - `<AgentApprovalTable />`: Fetches pending emails (Agent B5), handles Approve/Reject mutations.
  - `<SystemMetricsGrid />`: Fetches and renders analytical data.
- **Shared Components:**
  - `<StatCard />`
  - `<KanbanColumn />`, `<KanbanCard />`
  - `<DataTable />`
  - `<Badge />` (Status indicators)

### Client Dashboard (External)
- **LayoutWrapper:** `<DashboardLayout variant="client" />`
- **Feature Components:**
  - `<ProjectTimeline />`: Fetches active project milestones.
  - `<TelemetryGrid />`: Fetches AI performance metrics specific to the client's tenant ID.
  - `<SupportTicketForm />`: Handles API submission for support requests.
- **Shared Components:**
  - `<ProgressTracker />`
  - `<MetricChart />` (Wrapper around Recharts/Chart.js)
  - `<FileDownloadList />`

---

## 4. Authentication

### Authentication Pages (`/login`, `/signup`)
- **LayoutWrapper:** `<AuthLayout />`
- **Feature Components (`features/auth`):**
  - `<LoginForm />`: Handles form state (React Hook Form), validation (Zod), and Supabase auth submission.
  - `<MagicLinkRequest />`: Alternative passwordless auth component.
- **Shared Components:**
  - `<FormField />` (Email, Password with visibility toggle)
  - `<Button variant="primary" />` (with loading state binding)
  - `<OAuthButton />` (Google/GitHub SSO)

---

## 5. Reusability Strategy

By strictly separating presentation components (Shared) from logic-bound components (Features), we achieve high reuse:
- The `<DataTable />` used in the Admin `AgentApprovalTable` is the exact same component used in the Client `SupportTicketList`.
- The `<ChatMessage />` component is identical whether rendered in the floating `GlobalChatWidget` or the full-screen `AI Chat Interface`.
- The `<InteractiveProposalCard />` can be rendered in the chat feed, sent via email link, or viewed in the Client Dashboard deliverables tab.
