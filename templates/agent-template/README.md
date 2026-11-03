# New Agent Project Template

Copy this folder as the starting point for a new agent. Replace placeholders and remove components the agent does not need.

## Suggested structure

```text
agent-name/
├── README.md
├── AGENTS.md
├── pyproject.toml
├── uv.lock
├── .gitignore
├── .env.example
├── agent-governance.yaml
├── src/
│   └── agent_name/
│       ├── __init__.py
│       ├── config.py
│       ├── main.py
│       └── services/
├── tests/
│   ├── unit/
│   ├── integration/
│   └── security/
├── docs/
│   ├── architecture.md
│   └── threat-model.md
└── .github/
    └── workflows/
        └── quality-security.yml
```

## Project rules

- Use a separate `uv` environment.
- Commit `pyproject.toml` and `uv.lock`.
- Do not commit `.env`, databases, logs, private knowledge, or generated user content.
- Add idempotency tests for operations that can be retried.
- Validate all model-generated structured output.
- Require approval for external side effects.
- Add a governance manifest and threat model.
- Run security checks and local AI review before PR creation.

## First implementation tasks

1. Replace the placeholder name.
2. Write the problem statement and non-goals.
3. Complete the governance manifest.
4. Draw the architecture and data flow.
5. Define acceptance criteria.
6. Implement a minimal vertical slice.
7. Add tests and scanners.
8. Document local setup and recovery.
