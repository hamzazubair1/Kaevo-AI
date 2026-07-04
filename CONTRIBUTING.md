# Contributing to Kaevo AI

Thank you for your interest in contributing to **Kaevo AI**! This document provides guidelines and standards for contributing to the project.

---

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How to Contribute](#how-to-contribute)
- [Branch Naming Convention](#branch-naming-convention)
- [Commit Message Convention](#commit-message-convention)
- [Pull Request Process](#pull-request-process)
- [Code Standards](#code-standards)
- [Documentation Standards](#documentation-standards)

---

## Code of Conduct

By contributing, you agree to abide by our [Code of Conduct](CODE_OF_CONDUCT.md). Please read it before participating.

---

## How to Contribute

### 1. Fork & Clone

```bash
git clone https://github.com/kaevo-ai/kaevo-ai.git
cd kaevo-ai
```

### 2. Create a Branch

```bash
git checkout -b feature/your-feature-name
```

### 3. Make Changes

- Follow the [Development Rules](DEVELOPMENT_RULES.md)
- Write clean, documented code
- Add tests where applicable

### 4. Commit & Push

```bash
git add .
git commit -m "feat: add your feature description"
git push origin feature/your-feature-name
```

### 5. Open a Pull Request

- Use the PR template
- Link related issues
- Request review from a team member

---

## Branch Naming Convention

| Type | Pattern | Example |
|------|---------|---------|
| Feature | `feature/short-description` | `feature/chatbot-widget` |
| Bug Fix | `fix/short-description` | `fix/api-timeout` |
| Hotfix | `hotfix/short-description` | `hotfix/login-crash` |
| Documentation | `docs/short-description` | `docs/api-endpoints` |
| Refactor | `refactor/short-description` | `refactor/auth-module` |
| Chore | `chore/short-description` | `chore/update-deps` |

---

## Commit Message Convention

We follow the [Conventional Commits](https://www.conventionalcommits.org/) specification:

```
<type>(<scope>): <description>

[optional body]
[optional footer]
```

### Types

| Type | Description |
|------|-------------|
| `feat` | A new feature |
| `fix` | A bug fix |
| `docs` | Documentation changes |
| `style` | Formatting, missing semicolons, etc. |
| `refactor` | Code restructuring without behavior change |
| `test` | Adding or updating tests |
| `chore` | Maintenance tasks |
| `perf` | Performance improvements |
| `ci` | CI/CD changes |

### Examples

```
feat(chatbot): add multi-language support
fix(api): resolve timeout on large payloads
docs(readme): update tech stack section
refactor(auth): extract token validation to utility
```

---

## Pull Request Process

1. **Fill out the PR template** completely
2. **Link related issues** using keywords (`Closes #123`)
3. **Ensure all checks pass** (linting, tests, build)
4. **Request at least one review** before merging
5. **Squash commits** into a clean history when merging
6. **Delete the branch** after merging

---

## Code Standards

- Follow the [Development Rules](DEVELOPMENT_RULES.md)
- Use TypeScript for frontend code
- Use Python with type hints for backend/AI code
- Maintain consistent formatting (`.editorconfig` enforced)
- Write meaningful variable and function names
- Keep functions small and focused (Single Responsibility Principle)

---

## Documentation Standards

- Document every new feature, service, or component
- Update relevant docs when modifying existing functionality
- Use clear, concise language
- Include code examples where helpful
- Keep documentation in the `docs/` directory

---

## Questions?

If you have questions about contributing, please reach out to the team or open a discussion issue.

---

**Thank you for helping build Kaevo AI! 🚀**
