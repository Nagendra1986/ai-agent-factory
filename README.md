# Local AI Agent Factory

A reusable, local-first platform for designing, building, testing, reviewing, operating, and maintaining multiple AI agents on an Apple Silicon Mac.

## The central design decision: platform first, agents later

This repository describes **two separate workstreams**:

1. **Build and validate the Agent Factory platform** as a reusable prerequisite. This includes the local model runtime, chat and coding workspaces, development toolchain, shared standards, security controls, verification pipeline, GitHub workflow, and operational practices.
2. **Build individual agents on top of the validated platform**, only when a use case is selected. Each agent gets its own requirements, governance, dependencies, runtime data, tests, and deployment lifecycle.

The platform must be useful even if no agent is built yet. No agent-specific assumptions, data, integrations, or credentials belong in the platform foundation. The LinkedIn Response Agent is a possible first consumer of the factory, not the purpose of the factory and not a prerequisite for installing it.

## Platform capabilities and proposed components

| Capability | Proposed component | Platform or agent layer? |
|---|---|---|
| Local model runtime | Ollama | Platform |
| General chat/design workspace | Open WebUI | Platform |
| Coding workspace | VS Code + official Cline extension | Platform/developer tool |
| Local model candidates | Llama/Qwen family; benchmark before standardising | Platform |
| Python/toolchain management | uv | Platform |
| Source control and collaboration | Git + GitHub | Platform |
| Deterministic quality/security checks | Ruff, pytest, Bandit, Gitleaks, pip-audit, Trivy | Platform standards; applied per project |
| Agent API | FastAPI, only if needed | Agent-specific optional |
| Agent UI | Streamlit, only if needed | Agent-specific optional |
| Stateful orchestration | LangGraph, only if justified | Agent-specific optional |
| Structured state | SQLite, per agent | Agent-specific optional |
| Retrieval/vector store | ChromaDB or alternative, only if needed | Agent-specific optional |
| Containers | Docker Compose for selected local services | Platform/agent-specific |

These are defaults, not a requirement to install every framework or use every component in every agent. Prefer the smallest architecture that meets the validated use case.

## Repository layout

```text
ai-agent-factory/
├── README.md
├── docs/                 # platform and agent lifecycle documentation
├── standards/            # reusable security, quality, and governance standards
├── templates/            # starting point for future agent projects
├── shared/               # reviewed, reusable code only
├── infrastructure/       # platform service definitions and setup scripts
├── .github/workflows/    # deterministic CI checks
└── .gitignore

# Keep private/local runtime state outside this repository:
~/AI-Lab-local/
├── data/                 # databases, vector stores, user content
├── knowledge/            # private knowledge bases and source documents
├── secrets/              # local credentials/configuration
├── logs/                 # local logs
├── backups/              # encrypted local backups
└── models/               # optional model artifacts not managed by Ollama
```

The exact local directory can be adjusted, but runtime state must not be stored in a public Git repository. Docker named volumes and Ollama's model storage are also local runtime state.

## Read and implement in this order

### Workstream A — build the reusable platform

1. `docs/00-decisions-and-scope.md` — principles and boundaries.
2. `docs/01-reference-architecture.md` — platform layers and agent boundary.
3. `docs/02-mac-local-environment.md` — one-time platform setup.
4. `docs/03-ai-workspaces.md` — Open WebUI, VS Code, Cline, and local model workflow.
5. `docs/05-governance-and-risk.md` and `docs/06-security-and-code-review.md` — platform-wide guardrails.
6. `docs/07-github-and-pull-requests.md` — public repository and CI controls.
7. `docs/08-operations-and-maintenance.md` — backup, upgrades, and operations.
8. `docs/platform-acceptance-checklist.md` — prove the platform works before building an agent.

### Workstream B — build agents only after platform acceptance

1. Use `docs/04-agent-development-lifecycle.md`.
2. Copy `templates/agent-template/` for each new agent.
3. Complete its own requirements, data-flow, threat model, governance manifest, tests, and acceptance criteria.
4. Keep agent runtime data and private knowledge local and separate from the public source repository.

## Status and evidence

This repository is a documented target design and working checklist, not proof that all components are installed. Mark a component or phase complete only after the stated verification has been executed and its evidence recorded. Record versions and actual command results; do not infer success from a file existing.
