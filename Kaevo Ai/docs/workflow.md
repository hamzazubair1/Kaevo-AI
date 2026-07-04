# Kaevo AI — Team Workflow & Development Process

> How we work, communicate, and deliver at Kaevo AI.

---

## Development Workflow

### Git Flow

```
main (production)
 └── develop (integration)
      ├── feature/chatbot-widget
      ├── feature/api-auth
      ├── fix/timeout-error
      └── docs/api-endpoints
```

### Workflow Steps

1. **Pick a task** from the project board
2. **Create a branch** from `develop` using naming conventions
3. **Develop** the feature/fix with regular commits
4. **Push** and open a **Pull Request** to `develop`
5. **Code review** by at least one team member
6. **Merge** after approval and passing CI
7. **Deploy** `develop` → staging for testing
8. **Release** `develop` → `main` for production

---

## Sprint Cycle

| Day | Activity |
|-----|----------|
| **Monday** | Sprint planning, task assignment |
| **Daily** | Async standup (Slack/Discord update) |
| **Wednesday** | Mid-week sync (15 min) |
| **Friday** | Demo + retrospective |

### Standup Format
```
✅ Yesterday: What I completed
🔨 Today: What I'm working on
🚧 Blockers: What's blocking me
```

---

## Communication Standards

| Channel | Purpose | Response Time |
|---------|---------|---------------|
| **Slack/Discord** | Day-to-day communication | < 4 hours |
| **GitHub Issues** | Bug reports, feature requests | < 24 hours |
| **GitHub PRs** | Code review requests | < 24 hours |
| **Email** | Client communication, formal requests | < 24 hours |
| **Meetings** | Syncs, planning, demos | Scheduled |

### Rules
- Default to async communication
- Use threads to keep conversations organized
- Tag relevant people, don't broadcast everything
- Document decisions in writing (not just verbal)

---

## Project Management

### Tools
- **GitHub Projects** — Task tracking, sprint boards
- **GitHub Issues** — Bug tracking, feature requests
- **Notion / Docs** — Documentation, meeting notes

### Task Lifecycle

```
Backlog → To Do → In Progress → Review → Done
```

### Task Prioritization

| Priority | Label | Description |
|----------|-------|-------------|
| P0 | 🔴 Critical | Production down, data loss, security breach |
| P1 | 🟠 High | Major feature blocked, client-impacting bug |
| P2 | 🟡 Medium | Important but not urgent |
| P3 | 🟢 Low | Nice to have, minor improvements |

---

## Code Review Standards

### Reviewer Checklist
- [ ] Code is correct and solves the stated problem
- [ ] Code is readable and well-structured
- [ ] No security vulnerabilities
- [ ] Tests are included and passing
- [ ] Documentation updated
- [ ] No unnecessary dependencies added
- [ ] Follows project architecture patterns
- [ ] Performance considerations addressed

### Review Etiquette
- Be specific in your feedback
- Suggest alternatives, not just problems
- Approve when it's "good enough" — perfection is the enemy of progress
- Use conventional comment prefixes:
  - `nit:` — Minor nitpick, non-blocking
  - `suggestion:` — A recommended improvement
  - `question:` — Seeking clarification
  - `issue:` — Must be addressed before merge

---

## Release Process

### Versioning
We follow [Semantic Versioning](https://semver.org/):
```
MAJOR.MINOR.PATCH
  │      │      └── Bug fixes, patches
  │      └── New features (backward compatible)
  └── Breaking changes
```

### Release Checklist
- [ ] All tests passing on `develop`
- [ ] Code review complete for all PRs
- [ ] CHANGELOG.md updated
- [ ] Documentation updated
- [ ] Merge `develop` → `main`
- [ ] Tag release with version number
- [ ] Deploy to production
- [ ] Verify production deployment
- [ ] Notify team and stakeholders

---

## Client Project Workflow

```
Lead → Discovery → Proposal → Contract → Kickoff → Development → Delivery → Support
```

1. **Lead** — Initial inquiry or outreach
2. **Discovery** — Understanding needs, goals, constraints
3. **Proposal** — Scope, timeline, pricing document
4. **Contract** — Agreement signed, payment received
5. **Kickoff** — Project start meeting, access setup
6. **Development** — Iterative development with weekly updates
7. **Delivery** — Final testing, handoff, training
8. **Support** — Post-launch support period

---

> **Remember: Consistency in process leads to consistency in results.**
