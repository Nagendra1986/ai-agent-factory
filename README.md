# Local AI Agent Factory

A local-first development environment for designing, building, testing, reviewing, and maintaining multiple AI agents on an Apple Silicon Mac.

## Goals

- Use local LLM inference by default through Ollama.
- Provide a conversational workspace for discussing agent designs.
- Use an AI coding assistant to implement projects in VS Code.
- Reuse shared standards, templates, and utilities across agents.
- Make idempotency, governance, security, testing, and documentation mandatory.
- Publish only sanitised, intentionally public source code to GitHub.
- Use pull requests and automated checks without requiring a second human approver.

## Platform at a glance

| Capability | Proposed component |
|---|---|
| Local model runtime | Ollama |
| General chat interface | Open WebUI |
| Coding assistant | VS Code + Roo Code |
| Primary local models | Llama 3.1 8B and Qwen3 8B (evaluate on device) |
| Python environment management | uv |
| Agent APIs | FastAPI when needed |
| Lightweight UI | Streamlit when needed |
| Stateful orchestration | LangGraph when justified |
| Structured persistence | SQLite |
| Retrieval | ChromaDB or another local vector store when justified |
| Quality and security | Ruff, Bandit, Gitleaks, pip-audit, Trivy |
| Tests | pytest |
| Source control and PRs | GitHub |
| CI | GitHub Actions |

The components are defaults, not mandatory dependencies for every agent. Prefer the smallest architecture that satisfies the use case.

## Repository layout

```text
ai-agent-factory/
├── README.md
├── docs/
├── standards/
├── templates/
│   └── agent-template/
├── shared/
│   └── utilities/
├── infrastructure/
│   ├── docker/
│   └── scripts/
├── .github/
│   └── workflows/
└── .gitignore
```

Keep each agent's private runtime data outside the public repository. An agent may have its own repository if its source code is suitable for public release.

## Start here

1. Read `docs/00-decisions-and-scope.md`.
2. Follow `docs/02-mac-local-environment.md`.
3. Configure the chat and coding interfaces using `docs/03-ai-workspaces.md`.
4. Read `docs/04-agent-development-lifecycle.md`.
5. Apply `docs/05-governance-and-risk.md` and `docs/06-security-and-code-review.md` to every agent.
6. Use `docs/07-github-and-pull-requests.md` for source control and CI.
7. Copy `templates/agent-template/` when starting a new agent.

## Status

This repository is the documented target design. Installation and validation are not complete until each step has been executed on the Mac and its checks have passed. Do not treat documentation as proof that a component is installed or working.
