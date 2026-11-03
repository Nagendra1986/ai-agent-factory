# 00 — Decisions and Scope

## Purpose

Establish one reusable local AI development platform rather than a one-off environment for a single LinkedIn assistant. The LinkedIn Response Agent is intended to be Agent #001.

## Confirmed hardware

- MacBook Pro, Apple M5
- 10 CPU cores
- 24 GB unified memory
- macOS 26.5 (as reported by the owner)

The machine is suitable for experimenting with quantised local models. Actual model speed and context capacity must be measured locally. Unified memory is shared by macOS, applications, and models; avoid loading multiple large models simultaneously.

## Operating principles

1. Local-first: use local inference and local storage by default.
2. Reusable: share standards and utilities, but isolate agent dependencies and runtime data.
3. Human-controlled: AI can draft, analyse, test, and recommend. The owner decides what to merge, release, or publish externally.
4. Secure by default: least privilege, input validation, secret hygiene, auditability, and safe tool boundaries.
5. Idempotent: setup, database initialisation, retries, and state transitions must be safe to repeat.
6. Evidence-based: never claim a test or scan passed unless it actually ran and produced a successful result.
7. Proportionate: do not introduce multi-agent orchestration, a vector database, or a web API unless the use case needs it.

## Proposed tools

- Ollama for local model serving.
- Open WebUI for conversational interaction with local models.
- VS Code + Roo Code for project-scoped AI-assisted development.
- uv for Python versions, virtual environments, and dependency management.
- Git and GitHub for source control and pull requests.
- pytest, Ruff, Bandit, Gitleaks, pip-audit, and Trivy for verification.
- FastAPI, Streamlit, LangGraph, SQLite, and ChromaDB are optional building blocks.

Tool versions, model tags, licensing, and feature availability should be checked at installation time and recorded in the repository.

## GitHub visibility decision

The owner is working alone and prefers AI-assisted review rather than a second human approver. Public GitHub repositories are acceptable for reusable source code that the owner intentionally chooses to publish.

Never publish:
- credentials, tokens, cookies, private keys, or `.env` files;
- personal knowledge bases, private prompts, private messages, or personal datasets;
- local databases, logs, conversation history, or generated user content;
- proprietary employer/client information;
- personal or sensitive information;
- any content whose licence does not permit redistribution.

A public repository is public to everyone. `.gitignore` is not a privacy boundary and does not remove files already committed or data in Git history.

Keep runtime data and private knowledge outside the public repository. If an agent's source itself should not be public, keep that agent in a private repository or local-only workspace.

## Review and approval model

- No second human approver is required.
- Every change should go through a pull request into `main`.
- Automated CI checks are required.
- Local AI review is an advisory independent-scrutiny layer.
- The owner reviews findings and makes the merge/release decision.
- AI review must never auto-approve, auto-merge, or publish external actions.

Two local models can provide different perspectives but are not independent auditors and can share blind spots. Deterministic tests and security scanners remain mandatory.

## Important network boundary

GitHub-hosted Actions runners cannot access the Mac's `localhost` Ollama service. Keep local AI code review on the Mac before pushing or creating the PR. Do not expose Ollama or a self-hosted Mac runner to untrusted public pull-request code.

## Non-goals for the initial platform

- Fully autonomous publishing or messaging on third-party platforms.
- Unrestricted shell, filesystem, or browser access for agents.
- Exposing local services to the public internet.
- Installing every agent framework globally.
- Treating an LLM review as a security guarantee.
