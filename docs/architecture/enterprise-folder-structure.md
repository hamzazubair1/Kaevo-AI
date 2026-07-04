# Kaevo AI — Enterprise Folder Structure

> Repository organization reflecting the Day 2 Architecture (System 1 + System 2).

The repository follows a monorepo structure designed to gracefully separate concerns between the inbound platform, outbound engine, shared infrastructure, and documentation.

```text
Kaevo Ai/
├── .github/                        # GitHub Actions, CI/CD, Issue/PR templates
│
├── apps/                           # User-facing applications
│   ├── website/                    # Next.js Landing Site + Chat Widget (System 1)
│   ├── dashboard-admin/            # Admin/Sales internal portal (System 1 & 2)
│   └── dashboard-client/           # Client-facing portal (System 1)
│
├── services/                       # Backend microservices
│   ├── api-gateway/                # FastAPI routing, auth, rate limiting
│   ├── crm-service/                # Unified CRM logic and DB access
│   └── email-service/              # Outreach and transactional email sender
│
├── ai-engine/                      # The core AI runtime and agents
│   ├── core/
│   │   ├── llm-gateway/            # Model routing (OpenAI, Anthropic)
│   │   ├── memory-manager/         # Short/long-term context management
│   │   └── tools/                  # Shared agent tools (search, scrape, db)
│   │
│   ├── system-1-agents/            # Inbound Agents
│   │   ├── a1-receptionist/
│   │   ├── a2-qualification/
│   │   └── a3-proposal-gen/
│   │
│   └── system-2-agents/            # Outbound Agents
│       ├── b1-discovery/
│       ├── b2-website-analysis/
│       ├── b3-opportunity-scoring/
│       ├── b4-personalized-proposal/
│       └── b5-outreach-email/
│
├── packages/                       # Shared internal libraries
│   ├── db-schema/                  # Prisma/SQLAlchemy models (Unified DB)
│   ├── ui-components/              # Shared React components (Tailwind/Shadcn)
│   ├── config/                     # Shared ESLint, TSConfig, Prettier
│   └── types/                      # Shared TypeScript interfaces
│
├── docs/                           # Enterprise Documentation
│   ├── architecture/               # System designs, data models, security
│   ├── agents/                     # Agent specifications and registry
│   └── ...                         # Brand guidelines, SOPs, roadmaps
│
├── infrastructure/                 # IaC and deployment config
│   ├── terraform/                  # Cloud resource definitions
│   └── docker/                     # Compose files for local dev
│
├── scripts/                        # Internal tooling
└── README.md
```

## Structure Philosophy

1. **`apps/` vs `services/`:** `apps/` contains frontends (Next.js). `services/` contains backends (FastAPI/Node).
2. **`ai-engine/` isolation:** All AI logic is isolated from standard web logic. The web apps talk to the API Gateway, which delegates to the AI Engine.
3. **`packages/` sharing:** UI components are shared between the website and dashboards. Database schemas are shared between the API and the AI Engine.
4. **Scalability:** This monorepo layout (e.g., using Turborepo) allows easy extraction of services into separate repositories later if required in Phase 4/5.
