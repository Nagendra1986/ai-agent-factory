# 00 — Decisions, Scope, and Platform-First Strategy

## Purpose

Build one reusable local AI Agent Factory. The factory is the product of the initial work; individual agents are future workloads that consume it.

**Platform setup must not depend on the LinkedIn Response Agent or any other specific agent.** Do not create agent-specific prompts, data stores, API credentials, integrations, or workflows as part of the platform prerequisite. Those belong to a separately approved agent project after platform acceptance.

## Two-layer delivery model

### Layer 1: Agent Factory platform (first)

Establish and validate:
- local model serving and model inventory;
- conversational design workspace;
- project-scoped AI coding workspace;
- development toolchain and isolated dependency management;
- reusable repository standards and agent templates;
- deterministic testing and security scanning;
- local AI code-review workflow;
- GitHub PR and CI controls;
- local runtime-data separation, backup, recovery, and maintenance.

The platform should be demonstrably useful for a blank sample project. It must not contain assumptions about a specific agent's business logic or personal data.

### Layer 2: individual agents (later, on demand)

Each agent is a separate project/workload with its own problem statement, scope, data classification, model choice, tools, permissions, persistence, dependencies, tests, governance manifest, deployment, and lifecycle. Agents may reuse platform standards and approved shared libraries; they do not share secrets or mutable runtime databases by default.

The LinkedIn Response Agent is a candidate Agent #001 only after the factory passes its acceptance checklist. It is not part of the platform installation prerequisite.

## Confirmed hardware

- MacBook Pro, Apple M5
- 10 CPU cores
- 24 GB unified memory
- macOS 26.5 (owner-reported)

Benchmark models on this machine. Unified memory is shared by macOS, applications, and models; avoid loading multiple large models simultaneously.

## Operating principles

1. **Local-first:** local inference and local storage by default; document any network egress.
2. **Reusable:** platform standards are shared; agent dependencies and runtime state are isolated.
3. **Human-controlled:** the owner decides what to merge, release, publish, or execute externally.
4. **Secure by default:** least privilege, approval gates, input validation, secret hygiene, auditability, and safe tool boundaries.
5. **Idempotent:** setup, initialisation, retries, and state transitions are safe to repeat.
6. **Evidence-based:** never claim a test or scan passed unless it ran and produced a successful result.
7. **Proportionate:** install optional frameworks only when a validated requirement needs them.
8. **Platform/agent separation:** platform prerequisites remain agent-neutral.

## Proposed platform tools

- Ollama: local model serving.
- Open WebUI: conversational design and model experimentation.
- VS Code + official Cline extension: project-scoped coding assistant.
- uv: Python versions, virtual environments, and dependency management.
- Git/GitHub: source control, PRs, and CI.
- Ruff, pytest, Bandit, Gitleaks, pip-audit, Trivy: deterministic quality/security checks.
- Docker Desktop/Compose: selected local services, with persistent local volumes.

FastAPI, Streamlit, LangGraph, SQLite, ChromaDB, and other agent frameworks are optional and are selected per agent, not installed as mandatory platform prerequisites.

## GitHub visibility and data boundary

Public GitHub may contain sanitised reusable platform source, documentation, templates, and workflows that the owner intentionally publishes.

Never publish:
- credentials, tokens, cookies, private keys, `.env` files, or local configuration containing secrets;
- private knowledge bases, personal prompts, messages, or datasets;
- local databases, vector stores, logs, chat histories, or generated user content;
- employer/client proprietary information or sensitive information;
- data or content whose licence does not permit redistribution.

`.gitignore` is a safety net, not a privacy boundary. It does not remove previously committed files or Git history. Runtime data should live outside the repository, for example under `~/AI-Lab-local/`, or in explicitly local Docker volumes/Ollama storage.

## Review and approval model

- No second human approver is required.
- Changes use a PR into `main` and required deterministic CI checks.
- Local AI review is advisory and read-only.
- The owner reviews diffs, test/scan evidence, and AI findings before merging.
- AI must not auto-approve, auto-merge, or perform external publishing actions.
- Two models may offer different perspectives but are not independent auditors.

## Network boundary

GitHub-hosted Actions cannot access Ollama on the Mac's `localhost`. Keep local model-based review on the Mac. Do not expose Ollama publicly or run untrusted public PR code on a privileged self-hosted Mac runner.

## Non-goals for the platform foundation

- Building or deploying a LinkedIn agent as part of platform setup.
- Fully autonomous publishing or messaging.
- Unrestricted shell, filesystem, or browser access.
- Exposing local services to the public internet.
- Installing every agent framework globally.
- Treating an LLM review as a security guarantee.
