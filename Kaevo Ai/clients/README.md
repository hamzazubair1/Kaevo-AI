# Clients

> Client project files and resources. Each client gets a dedicated subdirectory.

## Structure

```
clients/
├── [client-name]/
│   ├── docs/           # Client-specific documentation
│   ├── assets/         # Client-provided assets
│   ├── deliverables/   # Completed deliverables
│   └── README.md       # Client project overview
└── README.md
```

## Rules

1. **Privacy** — Never commit sensitive client data (credentials, PII)
2. **Organization** — Each client gets their own directory
3. **Documentation** — Every project must have a README
4. **Cleanup** — Archive completed projects quarterly

## Creating a New Client Directory

```bash
mkdir -p clients/[client-name]/{docs,assets,deliverables}
```

> ⚠️ **Important:** Client credentials and sensitive data must NEVER be committed to Git. Use `.env` files and add them to `.gitignore`.
