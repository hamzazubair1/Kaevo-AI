# Changelog

All notable changes to **Kaevo AI** will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html) for software, combined with independent AI Engine versioning (`AI-vX.Y.Z`).

For details on the release management architecture, see [Release Management Guidelines](docs/architecture/release-management.md).

---

## [Unreleased]
**AI Engine Version:** AI-v0.4.0 (Draft)

### 🚨 Breaking Changes / Migration Notes
- Shift from single-layer changelog to Multi-Layer Enterprise Release format.

### 🔷 Layer 1: Product (SaaS & UI)
#### Added
- **Day 9: Sprint 1 (MVP) Implementation Plan**
  - Stripped architecture down to the absolute minimal 7-day build path.
  - Defined strict MVP scope (Login, Chat, Single Agent, 3 DB Tables).
  - Outlined step-by-step build order and binary success criteria.
- **Day 8: MVP Build Plan & Roadmap**
  - Finalized Tech Stack Lock (Next.js, Vercel AI SDK, Supabase).
  - Defined physical Turborepo codebase structure.
  - Drafted the 14-day execution roadmap to build Phase 1 MVP.
  - Specified API endpoints, database schemas (Prisma/SQL), and CI/CD pipelines.
- **Day 7: Full System Integration & MVP Scope**
  - Defined the Phase 1 MVP Scope (Standalone Chat, Admin Pipeline, Auth).
  - Outlined the deployment architecture (Vercel Edge, Supabase, Upstash, Railway Workers).
- **Day 4: Frontend Architecture Blueprint**
  - Design system to Component mapping strategy
  - Scalable Next.js feature-sliced folder structure (`app/`, `features/`, `components/`)
  - Page-to-component rendering breakdowns
  - State management strategy (React Query vs Zustand vs Local)
  - Data flow architecture (CQRS-lite, SSE streaming for chat)
  - API integration design (Axios interceptors, Zod validation)
  - SaaS scalability readiness (Multi-tenancy, RBAC, i18n)
- **Day 3: UI/UX & Design System**
  - Complete Design System (Colors, Typography, UI Components, Animation)
  - Reusable Design Tokens mapped to Dark Theme variables
  - Page-by-page UX Blueprints for the public website
  - Dashboard UX specs for Admin and Client portals
  - Responsive design guidelines (breakpoints and layout shifts)
  - Accessibility standards (WCAG, contrast, screen reader ARIA)
- **Day 1: Project Foundation**
  - Initial project folder tree structure
  - Core documentation framework (Mission, Roadmaps, Tech Stack)
  - Branding structure

### 🧠 Layer 2: AI Intelligence
#### AI Updates
- **Day 5: AI Agent Ecosystem & Orchestration System**
  - Architected the Multi-Agent AI Operating System (The Brain layer).
  - Defined 6 specialized Agent Guilds (Core, Business, Workflow, Data, Control, Tool).
  - Designed the MASTER Orchestrator routing, conflict resolution, and fallback logic.
  - Established a Hybrid Vector + Graph Memory Architecture for strict tenant isolation and long-term context retention.
  - Outlined the event-driven communication protocol and agent lifecycle management.
- **Day 4: AI Interaction Layer**
  - Engineered the frontend Streaming UI logic for AI responses
  - Architected the Tool-based rendering engine (rendering interactive cards from LLM JSON payloads)
- **Day 2: AI Agent Registry & Specifications**
  - Defined System 1 Agents: AI Receptionist, AI Qualification Assistant, AI Proposal Generator
  - Defined System 2 Agents: Business Discovery, Website Analysis, Opportunity Scoring, Personalized Proposal, Outreach Email
  - Established unified agent specification template (I/O, memory, tools, permissions)

### ⚙️ Layer 3: Infrastructure
#### Infra
- **Day 7: Full System Integration**
  - Designed the end-to-end Request Lifecycle mapping UI events to AI background execution.
  - Defined the Real-Time Chat System streaming architecture (SSE mechanics).
- **Day 6: Backend Architecture & AI Orchestration Integration**
  - Architected the Modular Monolith backend (scaling to Microservices) for global SaaS delivery.
  - Designed the robust Task Queue System (Redis/BullMQ) for asynchronous AI agent execution and failovers.
  - Defined the Real-Time AI Execution Layer using Server-Sent Events (SSE) and WebSockets.
  - Established Multi-Tenant logical isolation via PostgreSQL Row Level Security (RLS).
  - Integrated the Day 5 Memory Architecture (Vector/Graph) with backend Retrieval pipelines.
  - Charted horizontal scalability rules (Kubernetes autoscale via Queue length) for 10,000+ users.
- **Day 4: Enterprise Release Architecture**
  - Upgraded tracking system to multi-layer, dual-evolution release standard
- **Day 2: System Architecture**
  - System 1 (AI Agency Platform MVP) monolithic architecture
  - System 2 (AI Outbound Growth Engine) asynchronous batch architecture
  - Enterprise data models and entity relationship diagrams for unified CRM
  - Monorepo folder structure design supporting backend/frontend/AI engine separation

### 🛡️ Layer 4: Security & Compliance
#### Security
- **Day 2: Security & Compliance Architecture**
  - Established CAN-SPAM and GDPR guidelines for the Outbound Engine
  - Defined Human-in-the-loop (HITL) approval gates for outbound email agents
  - Auth, API key, and RBAC strategies defined

---

## [0.0.1] - 2026-07-04
**AI Engine Version:** AI-v0.0.0

### 🔷 Layer 1: Product (SaaS & UI)
#### Added
- Initial repository initialization.
- Base README.md and `.gitignore` setup.
