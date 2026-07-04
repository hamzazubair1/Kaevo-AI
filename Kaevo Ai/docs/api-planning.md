# Kaevo AI — API Planning Document

> Architecture, endpoints, authentication, versioning, and standards for the Kaevo AI API.

---

## API Overview

The Kaevo AI API serves as the backbone for all products and services. It provides a unified interface for:
- AI agent management and interaction
- Client project management
- Automation orchestration
- Analytics and reporting
- User authentication and authorization

---

## Architecture

### API Design Principles
- **RESTful** — Follow REST conventions for predictability
- **JSON** — All request/response bodies in JSON
- **Versioned** — All endpoints versioned via URL prefix
- **Authenticated** — JWT-based authentication with API keys
- **Rate Limited** — Protect against abuse
- **Documented** — OpenAPI/Swagger specification

### Architecture Diagram

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   Frontend   │     │  Mobile App  │     │ Third Party  │
│   (Next.js)  │     │   (Future)   │     │ Integrations │
└──────┬───────┘     └──────┬───────┘     └──────┬───────┘
       │                     │                     │
       └─────────────────────┼─────────────────────┘
                             │
                    ┌────────▼────────┐
                    │   API Gateway   │
                    │  (Rate Limit,   │
                    │   Auth, CORS)   │
                    └────────┬────────┘
                             │
              ┌──────────────┼──────────────┐
              │              │              │
       ┌──────▼──────┐ ┌────▼─────┐ ┌──────▼──────┐
       │   Agent     │ │  User    │ │  Analytics  │
       │   Service   │ │  Service │ │   Service   │
       └──────┬──────┘ └────┬─────┘ └──────┬──────┘
              │              │              │
       ┌──────▼──────────────▼──────────────▼──────┐
       │              Database Layer                │
       │         (PostgreSQL + Redis)               │
       └───────────────────────────────────────────┘
```

---

## API Versioning

```
Base URL: https://api.kaevo.ai/v1/
```

| Version | Status | Description |
|---------|--------|-------------|
| `v1` | 🟢 Active | Current stable version |
| `v2` | 📋 Planned | Future version with breaking changes |

### Versioning Rules
- Breaking changes require a new version
- Old versions supported for minimum 6 months after deprecation
- Non-breaking additions to existing version are allowed

---

## Authentication

### Methods

| Method | Use Case |
|--------|----------|
| **JWT Token** | User authentication (web/mobile) |
| **API Key** | Server-to-server, integrations |
| **OAuth 2.0** | Third-party app authorization (future) |

### JWT Flow

```
POST /v1/auth/login
{
  "email": "user@example.com",
  "password": "secure_password"
}

→ Response:
{
  "access_token": "eyJhbG...",
  "refresh_token": "eyJhbG...",
  "expires_in": 3600
}
```

### API Key Usage
```
GET /v1/agents
Authorization: Bearer ka_live_xxxxxxxxxxxx
```

---

## Endpoint Design

### Naming Conventions
- Use **plural nouns** for resources: `/agents`, `/projects`, `/users`
- Use **kebab-case** for multi-word resources: `/ai-agents`, `/case-studies`
- Use HTTP verbs for actions: GET, POST, PUT, PATCH, DELETE
- Nest related resources: `/projects/:id/agents`

### Planned Endpoints

#### Authentication (`/v1/auth/`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/auth/register` | Register a new user |
| POST | `/auth/login` | Authenticate and get tokens |
| POST | `/auth/refresh` | Refresh access token |
| POST | `/auth/logout` | Revoke tokens |
| POST | `/auth/forgot-password` | Initiate password reset |

#### Users (`/v1/users/`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/users/me` | Get current user profile |
| PATCH | `/users/me` | Update current user profile |
| GET | `/users/:id` | Get user by ID (admin) |
| GET | `/users` | List all users (admin) |

#### AI Agents (`/v1/agents/`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/agents` | List all agents |
| POST | `/agents` | Create a new agent |
| GET | `/agents/:id` | Get agent details |
| PATCH | `/agents/:id` | Update agent configuration |
| DELETE | `/agents/:id` | Delete an agent |
| POST | `/agents/:id/chat` | Send message to agent |
| GET | `/agents/:id/conversations` | Get agent conversations |
| GET | `/agents/:id/analytics` | Get agent analytics |

#### Projects (`/v1/projects/`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/projects` | List all projects |
| POST | `/projects` | Create a new project |
| GET | `/projects/:id` | Get project details |
| PATCH | `/projects/:id` | Update project |
| DELETE | `/projects/:id` | Archive project |
| GET | `/projects/:id/agents` | List agents in project |

#### Automations (`/v1/automations/`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/automations` | List all automations |
| POST | `/automations` | Create automation |
| GET | `/automations/:id` | Get automation details |
| PATCH | `/automations/:id` | Update automation |
| POST | `/automations/:id/trigger` | Manually trigger automation |
| GET | `/automations/:id/logs` | Get execution logs |

---

## Response Format

### Success Response
```json
{
  "status": "success",
  "data": {
    "id": "agent_123",
    "name": "Customer Support Bot",
    "type": "conversational",
    "status": "active"
  },
  "meta": {
    "request_id": "req_abc123",
    "timestamp": "2026-07-04T12:00:00Z"
  }
}
```

### Error Response
```json
{
  "status": "error",
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid request body",
    "details": [
      {
        "field": "email",
        "message": "Must be a valid email address"
      }
    ]
  },
  "meta": {
    "request_id": "req_abc456",
    "timestamp": "2026-07-04T12:00:00Z"
  }
}
```

### HTTP Status Codes

| Code | Meaning | Usage |
|------|---------|-------|
| 200 | OK | Successful GET, PATCH |
| 201 | Created | Successful POST |
| 204 | No Content | Successful DELETE |
| 400 | Bad Request | Validation errors |
| 401 | Unauthorized | Missing/invalid authentication |
| 403 | Forbidden | Insufficient permissions |
| 404 | Not Found | Resource doesn't exist |
| 409 | Conflict | Duplicate resource |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Internal Server Error | Unexpected server error |

---

## Rate Limiting

| Tier | Requests/Min | Requests/Day |
|------|-------------|-------------|
| Free | 30 | 1,000 |
| Pro | 120 | 10,000 |
| Enterprise | 600 | 100,000 |

Rate limit headers included in every response:
```
X-RateLimit-Limit: 120
X-RateLimit-Remaining: 115
X-RateLimit-Reset: 1720094400
```

---

## Implementation Phases

| Phase | Scope | Timeline |
|-------|-------|----------|
| Phase 1 | Auth, Users, basic CRUD | Month 2–3 |
| Phase 2 | Agents, Chat, Projects | Month 3–5 |
| Phase 3 | Automations, Analytics | Month 5–7 |
| Phase 4 | Public API, SDKs, Webhooks | Month 8+ |

---

> **API documentation will be auto-generated from OpenAPI spec and hosted at `docs.kaevo.ai/api`**
