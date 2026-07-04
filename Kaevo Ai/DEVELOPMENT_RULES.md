# Kaevo AI — Development Rules & Engineering Standards

> These rules are non-negotiable. Every team member, contributor, and AI agent must follow them.

---

## Table of Contents

- [1. Document Everything](#1-document-everything)
- [2. Automate Repetitive Work](#2-automate-repetitive-work)
- [3. Build Reusable Systems](#3-build-reusable-systems)
- [4. Use Git for Every Change](#4-use-git-for-every-change)
- [5. Follow Clean Architecture](#5-follow-clean-architecture)
- [6. Security First](#6-security-first)
- [7. Test Before You Ship](#7-test-before-you-ship)
- [8. Code Review is Mandatory](#8-code-review-is-mandatory)
- [9. Performance Matters](#9-performance-matters)
- [10. AI-First Thinking](#10-ai-first-thinking)

---

## 1. Document Everything

> _"If it's not documented, it doesn't exist."_

- **Every feature** must have documentation before it's considered complete
- **Every API endpoint** must be documented with request/response examples
- **Every decision** must be recorded with reasoning (ADRs — Architecture Decision Records)
- **Every SOP** must be written so that anyone can follow it without prior context
- **Update docs** when you change code — stale documentation is worse than none

### Standards
- Use Markdown for all documentation
- Store docs in the `docs/` directory
- Include code examples wherever possible
- Write for someone who has never seen the project before

---

## 2. Automate Repetitive Work

> _"If you do it twice, automate it."_

- **Deployments** must be automated via CI/CD pipelines
- **Testing** must run automatically on every push
- **Code formatting** must be enforced by automated tools
- **Client onboarding** steps should be templated and scripted
- **Reporting** should be generated, not manually assembled

### Standards
- Use GitHub Actions for CI/CD
- Use pre-commit hooks for linting and formatting
- Build internal tools in `internal-tools/` for recurring tasks
- Document every automation in `automations/`

---

## 3. Build Reusable Systems

> _"Don't build it once — build it to be used a thousand times."_

- **Components** must be modular and self-contained
- **Functions** must do one thing and do it well (Single Responsibility)
- **Templates** must be created for any document, email, or proposal used more than once
- **Services** must be designed as composable building blocks
- **AI Agents** must be configurable, not hard-coded

### Standards
- Extract shared logic into `src/lib/` or `src/utils/`
- Store reusable templates in `templates/`
- Design APIs to be consumed by multiple frontends
- Use configuration files over hard-coded values

---

## 4. Use Git for Every Change

> _"No commit is too small. No push is too frequent."_

- **Every change** — code, docs, config — must go through Git
- **Branches** must follow the naming convention (see [CONTRIBUTING.md](CONTRIBUTING.md))
- **Commits** must follow Conventional Commits format
- **Pull Requests** are mandatory for all changes to `main`
- **Never force push** to shared branches

### Standards
- Commit frequently with meaningful messages
- Keep PRs focused and small (< 400 lines when possible)
- Use feature branches for all new work
- Protect the `main` branch — no direct pushes

### Branch Strategy
```
main          ← Production-ready code
├── develop   ← Integration branch
├── feature/* ← New features
├── fix/*     ← Bug fixes
├── hotfix/*  ← Emergency fixes
└── docs/*    ← Documentation updates
```

---

## 5. Follow Clean Architecture

> _"Architecture is about the important stuff. Whatever that is."_ — Martin Fowler

- **Separation of Concerns** — UI, business logic, and data access are separate layers
- **Dependency Inversion** — High-level modules don't depend on low-level modules
- **Interface Segregation** — No client should depend on methods it doesn't use
- **DRY** — Don't Repeat Yourself
- **KISS** — Keep It Simple, Stupid

### Project Architecture Layers
```
┌─────────────────────────────┐
│       Presentation          │  ← UI, API routes, controllers
├─────────────────────────────┤
│       Application           │  ← Use cases, business logic
├─────────────────────────────┤
│       Domain                │  ← Entities, models, interfaces
├─────────────────────────────┤
│       Infrastructure        │  ← Database, external APIs, services
└─────────────────────────────┘
```

### Standards
- One file, one purpose
- Maximum function length: 30 lines (guideline, not hard rule)
- Maximum file length: 300 lines (guideline, not hard rule)
- Use meaningful names — code should read like prose
- Prefer composition over inheritance

---

## 6. Security First

> _"Security is not a feature — it's a requirement."_

- **Never commit secrets** — API keys, passwords, tokens → use `.env` files
- **Validate all inputs** — both client-side and server-side
- **Use HTTPS everywhere** — no exceptions
- **Implement auth properly** — JWT with refresh tokens, proper session management
- **Sanitize user data** — prevent XSS, SQL injection, CSRF

### Standards
- Add `.env` files to `.gitignore`
- Use environment variables for all configuration
- Conduct security reviews for client-facing features
- Follow OWASP Top 10 guidelines
- Encrypt sensitive data at rest and in transit

---

## 7. Test Before You Ship

> _"Untested code is broken code — you just don't know it yet."_

- **Write tests** for all business-critical logic
- **Run tests** before every PR merge
- **Integration tests** for API endpoints
- **E2E tests** for critical user flows
- **Manual QA** for UI/UX before client delivery

### Standards
- Minimum 70% code coverage for core modules
- Use Jest/Vitest for frontend, pytest for backend
- CI must block merges on test failures
- Write tests alongside code, not after

---

## 8. Code Review is Mandatory

> _"Two pairs of eyes are always better than one."_

- **Every PR** must be reviewed by at least one team member
- **Review for** correctness, readability, performance, and security
- **Be constructive** — suggest improvements, don't just criticize
- **Respond promptly** — reviews should happen within 24 hours

### Review Checklist
- [ ] Code is readable and well-documented
- [ ] No security vulnerabilities introduced
- [ ] Tests are included for new functionality
- [ ] No unnecessary dependencies added
- [ ] Follows the project architecture patterns
- [ ] Documentation updated if needed

---

## 9. Performance Matters

> _"Fast software is a feature."_

- **Optimize database queries** — avoid N+1 problems
- **Lazy load** assets and components where possible
- **Cache aggressively** — API responses, computed values, static assets
- **Monitor performance** — set up alerts for degradation
- **Profile before optimizing** — don't guess, measure

### Standards
- Page load time target: < 2 seconds
- API response time target: < 200ms (p95)
- Lighthouse score target: 90+ on all metrics
- Bundle size monitoring for frontend

---

## 10. AI-First Thinking

> _"If AI can do it, AI should do it."_

- **Leverage AI tools** for code generation, testing, and documentation
- **Build AI agents** for repetitive internal tasks
- **Use LLMs** for content generation, summarization, and analysis
- **Automate with AI** before hiring for a task
- **Stay current** — evaluate new AI tools and models monthly

### Standards
- Document all AI prompts and agent configurations
- Version control AI agent definitions
- Monitor AI agent performance and accuracy
- Build fallback mechanisms for AI-dependent systems
- Maintain human oversight for critical decisions

---

## Enforcement

These rules are enforced through:

1. **Automated tooling** — Linters, formatters, CI checks
2. **Code reviews** — PRs cannot be merged without passing review
3. **Team culture** — We hold each other accountable
4. **Regular audits** — Monthly reviews of code quality and standards adherence

---

## Exceptions

Exceptions to these rules require:

1. A documented reason in the PR description
2. Explicit approval from the project lead
3. A plan to resolve the exception within a defined timeline

---

> **Remember: We're not just building software — we're building a company. Every line of code, every document, and every decision is a brick in the foundation of Kaevo AI.**
