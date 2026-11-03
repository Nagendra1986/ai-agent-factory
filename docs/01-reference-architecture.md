# 01 — Reference Architecture

## Logical layers

```text
Human / Developer
    |
    +-- Open WebUI: chat, design discussion, prompt experiments
    |
    +-- VS Code + Roo Code: inspect/edit files, run tests with permission
              |
              v
        Agent Factory workspace
              |
    +---------+---------------------------+
    | Shared standards and templates      |
    | Agent-specific source and tests     |
    | Local-only data/configuration       |
    +---------+---------------------------+
              |
       Ollama local API
              |
       Local LLM models
              |
    Agent runtime (per project)
              |
    SQLite / optional vector store / APIs
```

## Separation of responsibilities

### Chat workspace

Open WebUI is for conversations, exploring ideas, drafting system prompts, and testing model behaviour. It is not automatically authorised to modify the Mac's files.

### Coding workspace

Roo Code in VS Code is the implementation assistant. It should operate in the active project workspace, use approval controls for terminal and file operations, and follow the repository's `AGENTS.md` and project rules.

### Model runtime

Ollama serves local models through its local API. Model choice should be evaluated against the task, memory use, latency, structured-output support, and tool-calling needs.

### Agent runtime

Each agent is an independent application with its own dependencies, configuration, tests, logs, and data location. Agents may use shared libraries from the factory, but must not share mutable databases or secrets by default.

## Suggested model roles

Initial experiments:
- Llama 3.1 8B: general chat, requirements analysis, response drafting, and review.
- Qwen3 8B: coding assistance and a second review perspective.

These are candidate models, not fixed requirements. Compare them on representative tasks before standardising. Run models sequentially on a 24 GB machine if memory pressure occurs.

## Optional components

- FastAPI: use when an agent needs an API or service boundary.
- Streamlit: use for a lightweight local UI.
- LangGraph: use when workflows need explicit state, branching, checkpoints, or human approval.
- SQLite: use for structured local state and audit records.
- ChromaDB: use only when semantic retrieval is needed and simple file/SQLite retrieval is insufficient.
- Docker: use for services that benefit from container isolation, such as Open WebUI.

## Local network and security

- Bind local dashboards to `127.0.0.1` unless remote access is deliberately designed and secured.
- Do not expose Ollama's API to the internet.
- Use named Docker volumes for persistent service data.
- Keep secrets out of source control.
- Do not run untrusted PR code on a self-hosted Mac runner.
