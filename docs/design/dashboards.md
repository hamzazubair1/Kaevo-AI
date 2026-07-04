# Kaevo AI — Dashboards UX Blueprint

> UX specifications for the internal Admin/Sales Dashboard (Systems 1 & 2) and the Client-facing Dashboard.

---

## 1. Admin Dashboard (Internal)

### Purpose
The command center for Kaevo AI staff to manage inbound leads (System 1), outbound campaigns (System 2), AI agent configurations, and client projects.

### Global Navigation
- **Layout:** Left vertical sidebar (collapsible).
- **Items:**
  - 📊 Overview (Metrics)
  - 📥 Pipeline (CRM / Kanban)
  - 🎯 Outbound (System 2 Engine)
  - 🤖 AI Agents (Management & Logs)
  - 🤝 Clients (Active Projects)
  - ⚙️ Settings

### Key Views

#### Overview (Home)
- **Top Metrics Row (KPIs):** Leads this week, Meetings booked, Active Outbound Campaigns, Revenue.
- **Charts:** 
  - Line chart: Inbound vs. Outbound lead volume over 30 days.
  - Funnel chart: Conversion rates (Discovered → Analyzed → Proposal → Won).
- **Recent Activity Feed:** Real-time log of AI agent actions (e.g., "Agent B4 generated a proposal for Acme Corp").

#### Pipeline (CRM)
- **Kanban Board:** Drag-and-drop columns (New, Qualified, Meeting, Proposal, Won).
- **Lead Card:** Shows Company Name, Lead Score (color-coded badge), Industry, and next task.
- **Detail Flyout:** Clicking a card opens a right-side drawer with full qualification data, AI analysis gaps, and chat transcripts.

#### AI Agents (Control Center)
- **Agent Grid:** Cards for Agents A1-A3 and B1-B5 showing active status (green dot), total tasks processed, and error rate.
- **Approval Queue:** A dedicated table for Agent A3 (Proposals) and Agent B5 (Emails) requiring human review.
  - **Action:** "Approve & Send", "Edit", or "Reject".

### UX Notes & States
- **Loading State:** Use skeleton loaders for the Kanban board and charts.
- **Empty State:** If the pipeline is empty, show a graphic: "No leads yet. Start an outbound campaign." with a CTA button.
- **Data Tables:** Must support sorting, filtering, and bulk actions (checkboxes on the left).

---

## 2. Client Dashboard (External)

### Purpose
A secure portal for active Kaevo AI clients to view project progress, interact with deliverables, and access support.

### Global Navigation
- **Layout:** Top horizontal navigation bar.
- **Items:**
  - 🏠 Dashboard (Summary)
  - 📈 Telemetry (AI Performance)
  - 📁 Deliverables (Files/Links)
  - 💬 Support (Chat/Tickets)
  - 💳 Billing

### Key Views

#### Dashboard (Home)
- **Project Progress:** A visual timeline or progress bar (e.g., "Phase 2: Agent Configuration").
- **Next Milestone:** Card highlighting what the Kaevo team is working on right now.
- **Recent Deliverables:** Quick links to recently uploaded documents or generated URLs.

#### Telemetry (Value Demonstration)
- **Purpose:** To visually prove the ROI of the deployed Kaevo AI solutions.
- **Widgets:**
  - "Conversations Handled by AI" (Counter)
  - "Estimated Hours Saved" (Counter based on benchmark metrics)
  - "Lead Capture Rate" (Line chart)

#### Support
- **Interface:** Integrated AI Support Agent (Tier 1) with seamless escalation to human ticketing.

### UX Notes & States
- **Tone:** Professional, reassuring, and highly polished.
- **Empty States (Pre-launch):** If an AI system isn't live yet, the Telemetry tab should show a beautiful "System Initializing..." graphic with a countdown or progress indicator.
- **Responsive:** Must be fully functional on mobile for CEOs checking stats on the go.
