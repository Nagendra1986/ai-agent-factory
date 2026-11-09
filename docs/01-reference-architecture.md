# 01 — Reference Architecture

## Architecture principle

The factory platform is a reusable developer capability. Agent applications are workloads built on top of it. Keep the platform operational and useful without any agent deployed.

## Logical view

```text
                    PLATFORM FOUNDATION
Human / Developer ------------------------------------+
   |                                                   |
   +-- Open WebUI: chat, requirements, prompt tests    |
   +-- VS Code + Cline: project-scoped implementation  |
   +-- Git/GitHub: source, PRs, CI                     |
   +-- Quality/security tools: deterministic checks    |
                         |                             |
                  Agent Factory repository             |
                  - standards and templates             |
                  - shared, reviewed utilities          |
                  - infrastructure definitions         |
                         |                             |
                  Ollama local API                      |
                         |                             |
                  Local model inventory                 |
                                                       |
                    AGENT WORKLOADS (LATER)             |
          +----------------+  +----------------+        |
          | Agent A        |  | Agent B        |  ...   |
          | own code       |  | own code       |        |
          | own deps       |  | own deps       |        |
          | own data       |  | own data       |        |
          | own governance |  | own governance|        |
          +----------------+  +----------------+        |
```

## Platform responsibilities

### Conversational workspace

Open WebUI supports design discussions, requirements analysis, prompt experiments, and model evaluation. It is not automatically authorised to modify project files. Its conversations and uploaded private materials are runtime data and must remain local/private.

### Coding workspace

VS Code with the official Cline extension supports repository inspection, planning, implementation, tests, and tool use. Restrict the workspace to the active repository. Keep approval controls enabled for file edits and terminal commands. The coding assistant is a developer tool, not a production agent runtime.

### Model runtime

Ollama serves local models through its local API. Evaluate model quality, context size, tool-calling support, latency, and memory use. Confirm that the selected provider is the local Ollama endpoint before submitting private source or prompts.

### Engineering control plane

Git/GitHub, CI, scanners, tests, templates, and governance rules provide repeatable engineering controls. GitHub-hosted CI runs deterministic checks; local AI review remains on the Mac unless a separately secured design is approved.

## Agent workload responsibilities (added later)

Each agent owns its business logic, dependencies, configuration, data stores, prompts, integrations, access permissions, audit records, tests, and operational lifecycle. Agent workloads may consume approved platform services and shared libraries but must not couple the factory to one use case.

## Suggested model candidates

Llama and Qwen family models may be evaluated for general chat, coding, and review. Model tags and context windows change; select based on actual benchmark results on the M5/24 GB machine. Do not treat a suggested model as a fixed platform dependency.

## Optional agent-level components

- FastAPI for an API boundary.
- Streamlit for a lightweight local UI.
- LangGraph for explicit state, branching, checkpoints, or human approval.
- SQLite for structured per-agent state.
- ChromaDB or another vector store only when semantic retrieval is required.
- External APIs/connectors only after a documented need, privacy review, and approval model.

## Local network and runtime-data security

- Bind local dashboards to `127.0.0.1` unless remote access is explicitly designed and secured.
- Do not expose Ollama's API to the internet.
- Use Docker named volumes or explicit host paths for persistent service data; document the exact location and backup method.
- Keep `.env`, databases, logs, chat history, private knowledge, and generated user content out of Git.
- A local model provider does not by itself guarantee that an extension or plugin makes no network requests. Review telemetry, MCP servers, browser tools, and integrations separately.
