# Kaevo AI — Technology Stack

> Complete technology stack documentation for all Kaevo AI systems.

---

## Stack Overview

```
Frontend ─── Next.js / React / TypeScript / Tailwind CSS
Backend  ─── Python (FastAPI) / Node.js
AI/ML    ─── LangChain / CrewAI / OpenAI / Vector Stores
Database ─── PostgreSQL / Redis / Pinecone
DevOps   ─── GitHub Actions / Vercel / Docker
Tools    ─── n8n / Make / ElevenLabs / Twilio
```

---

## Frontend

| Technology | Version | Purpose |
|------------|---------|---------|
| **Next.js** | 14+ | React framework with SSR/SSG, App Router |
| **React** | 18+ | Component-based UI library |
| **TypeScript** | 5+ | Type safety and developer experience |
| **Tailwind CSS** | 3+ | Utility-first CSS framework |
| **Framer Motion** | 11+ | Animation library |
| **Shadcn/ui** | Latest | Accessible component library |
| **Lucide Icons** | Latest | Icon library |
| **React Hook Form** | 7+ | Form handling |
| **Zod** | 3+ | Schema validation |

---

## Backend

| Technology | Version | Purpose |
|------------|---------|---------|
| **Python** | 3.11+ | Primary backend language for AI services |
| **FastAPI** | 0.100+ | High-performance Python API framework |
| **Node.js** | 20+ | JavaScript runtime for auxiliary services |
| **Pydantic** | 2+ | Data validation and settings management |
| **SQLAlchemy** | 2+ | Python ORM |
| **Alembic** | 1.12+ | Database migrations |
| **Celery** | 5+ | Distributed task queue |

---

## AI / Machine Learning

| Technology | Purpose |
|------------|---------|
| **LangChain** | LLM orchestration, chains, agents, RAG |
| **CrewAI** | Multi-agent orchestration |
| **AutoGen** | Autonomous agent conversations |
| **OpenAI API** | GPT-4, GPT-4o, Embeddings |
| **Anthropic API** | Claude models |
| **Google AI** | Gemini models |
| **Hugging Face** | Open-source models, transformers |
| **LlamaIndex** | Data indexing and retrieval |

---

## Vector Stores & Embeddings

| Technology | Purpose |
|------------|---------|
| **Pinecone** | Managed vector database (production) |
| **ChromaDB** | Local vector database (development) |
| **Weaviate** | Alternative vector store |
| **OpenAI Embeddings** | text-embedding-3-small/large |

---

## Database & Storage

| Technology | Purpose |
|------------|---------|
| **PostgreSQL** | Primary relational database |
| **Supabase** | Managed PostgreSQL + Auth + Storage |
| **Redis** | Caching, sessions, rate limiting, queues |
| **AWS S3** | Object/file storage |

---

## Voice AI

| Technology | Purpose |
|------------|---------|
| **ElevenLabs** | Voice synthesis (text-to-speech) |
| **Deepgram** | Speech-to-text transcription |
| **Twilio** | Telephony and phone number management |
| **VAPI** | Voice agent platform |

---

## Automation

| Technology | Purpose |
|------------|---------|
| **n8n** | Primary workflow automation (self-hosted) |
| **Make (Integromat)** | Visual workflow automation |
| **Zapier** | Simple integrations (client-facing) |
| **Custom Scripts** | Python/Node.js for bespoke automations |

---

## DevOps & Infrastructure

| Technology | Purpose |
|------------|---------|
| **GitHub** | Version control and collaboration |
| **GitHub Actions** | CI/CD pipelines |
| **Vercel** | Frontend hosting and edge functions |
| **Railway** | Backend service hosting |
| **Docker** | Containerization |
| **Docker Compose** | Local development environment |

---

## Monitoring & Analytics

| Technology | Purpose |
|------------|---------|
| **Sentry** | Error tracking and monitoring |
| **Vercel Analytics** | Web performance analytics |
| **Google Analytics 4** | Visitor analytics |
| **Posthog** | Product analytics (future) |
| **Uptime Robot** | Uptime monitoring |

---

## Development Tools

| Tool | Purpose |
|------|---------|
| **VS Code** | Primary code editor |
| **Cursor** | AI-assisted code editor |
| **Postman / Hoppscotch** | API testing |
| **Figma** | UI/UX design |
| **Notion** | Documentation and project management |
| **Linear** | Issue tracking (future) |

---

## Communication

| Tool | Purpose |
|------|---------|
| **Slack / Discord** | Team communication |
| **Google Meet / Zoom** | Video calls |
| **Loom** | Async video communication |
| **Email** | Client communication |

---

## Selection Criteria

We choose technologies based on:

1. **Developer Experience** — How fast can we build with it?
2. **Scalability** — Will it grow with us?
3. **Community** — Is there strong community support?
4. **Cost** — Is it affordable at our stage?
5. **Integration** — Does it play well with our stack?
6. **AI-Readiness** — Does it support AI/ML workflows?

---

> **Stack evolves with the company. This document is updated as technologies are adopted or replaced.**
