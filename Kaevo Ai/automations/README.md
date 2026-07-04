# Automations

> Workflow automations, scripts, and integration configurations.

## Structure

```
automations/
├── n8n/        # n8n workflow JSON exports
├── make/       # Make (Integromat) scenario exports
├── zapier/     # Zapier workflow documentation
├── custom/     # Custom Python/Node.js automation scripts
└── README.md
```

## Documentation

Each automation must include:
- Description of what it does
- Trigger conditions
- Input/output specifications
- Error handling notes
- Maintenance schedule

## Naming Convention

```
[category]-[action]-[target].[ext]

Examples:
client-onboarding-email.json
invoice-generate-monthly.py
lead-capture-crm-sync.json
```
