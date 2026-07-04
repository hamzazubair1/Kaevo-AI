# AI Agents

> AI agent configurations, prompts, workflows, and integrations.

## Structure

```
ai-agents/
├── agent-configs/   # YAML configuration files for each agent
├── prompts/         # System prompts and prompt templates
├── workflows/       # Agent workflow definitions
├── integrations/    # Third-party integration configurations
└── README.md
```

## Documentation

- See [AI Agents Documentation](../docs/ai-agents.md) for architecture and standards
- Every agent must have a configuration file in `agent-configs/`
- All prompts must be version-controlled in `prompts/`

## Creating a New Agent

1. Create a config file in `agent-configs/` following the template
2. Write system prompts in `prompts/`
3. Define workflow in `workflows/` if applicable
4. Document integrations in `integrations/`
5. Test thoroughly before deployment
