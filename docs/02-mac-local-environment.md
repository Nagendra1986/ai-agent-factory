# 02 — One-time Mac Platform Environment Setup

## Scope

This guide installs and validates the **agent-neutral platform foundation only**. It does not build, configure, or deploy a LinkedIn agent or any other agent. Agent-specific dependencies, prompts, data, integrations, and databases are selected later for each workload.

## Machine profile

Target machine: Apple M5 MacBook Pro, 24 GB unified memory, macOS 26.5 (owner-reported). Confirm current state before installing anything; do not reinstall tools that already work.

## 1. Inspect current installations

```bash
sw_vers
uname -m
brew --version
python3 --version
git --version
uv --version
docker --version
ollama --version
ollama list
code --version
```

A missing command means that component may not be installed or may not be on `PATH`. Verify before changing the machine.

## 2. Install base tools only when missing

Install Homebrew from its official instructions if absent. Then install only missing items:

```bash
brew update
brew install git uv jq
brew install --cask visual-studio-code
brew install --cask docker
```

Install Ollama using its official macOS installer: https://ollama.com/download
Open Docker Desktop once and complete first-run setup. Record installed versions in a local inventory.

## 3. Python foundation

```bash
uv python install 3.12
uv python list --installed
```

This provides a managed Python runtime. Do not install agent project dependencies globally; create an isolated `uv` environment for each future agent.

## 4. Local model runtime

Install Ollama and test one candidate model at a time. Candidate models are examples, not fixed requirements:

```bash
ollama list
ollama pull llama3.1:8b
ollama run llama3.1:8b
```

Exit with `/bye`. Evaluate quality, latency, context length, tool calling, and memory use before adding another model. Do not download multiple large models just because they appear in an old checklist.

Verify local API:

```bash
curl -fsS http://localhost:11434/api/tags
```

## 5. Create a local/private data root outside Git

Use a dedicated local directory separate from the public source repository:

```bash
mkdir -p ~/AI-Lab-local/{data,knowledge,secrets,logs,backups,models}
chmod 700 ~/AI-Lab-local ~/AI-Lab-local/{data,knowledge,secrets,logs,backups,models}
```

This is a suggested layout, not a requirement to move Ollama's managed model storage. Docker named volumes are also local runtime storage. Never copy private runtime data into the public repository.

## 6. Open WebUI (platform chat workspace)

Open WebUI is a platform-level chat/design interface, not a LinkedIn-agent component. Store its service configuration in the platform repository if desired, but store its secret and persistent data locally.

Create the local secret file without overwriting an existing key:

```bash
mkdir -p ~/AI-Lab/infrastructure/docker
cd ~/AI-Lab/infrastructure/docker
umask 077
if [ ! -f .env ]; then
  printf 'WEBUI_SECRET_KEY=%s\n' "$(openssl rand -hex 32)" > .env
fi
chmod 600 .env
```

Ensure the repository `.gitignore` excludes `.env` (and ideally `.env.*`, while allowing `.env.example`). Verify from the repository root:

```bash
git check-ignore -v infrastructure/docker/.env
```

Use the Compose example below in `infrastructure/docker/docker-compose.yml`. Keep the service bound to localhost and persist data in a Docker named volume:

```yaml
services:
  open-webui:
    image: ghcr.io/open-webui/open-webui:main # replace with a tested release tag/digest before long-term use
    container_name: ai-lab-open-webui
    restart: unless-stopped
    ports:
      - "127.0.0.1:3000:8080"
    environment:
      OLLAMA_BASE_URL: http://host.docker.internal:11434
      WEBUI_SECRET_KEY: ${WEBUI_SECRET_KEY}
    volumes:
      - open-webui-data:/app/backend/data

volumes:
  open-webui-data:
    name: ai-lab-open-webui-data
```

Validate before starting:

```bash
docker compose config
docker compose up -d
docker compose ps
docker compose logs --tail=50 open-webui
```

Open `http://localhost:3000` and complete initial setup. Do not use `docker compose down -v` for routine stopping; `-v` can remove persistent volumes. Pin a tested image version/digest before treating this as a stable service and document backup/restore.

## 7. VS Code + official Cline extension (coding workspace)

Install the extension named **Cline**, published by **Cline** (verified publisher). Avoid similarly named forks, unofficial builds, Chinese-localisation variants, and Nightly for the baseline setup. Official installation guide: https://docs.cline.bot/getting-started/installing-cline

Configure the model provider later in `docs/03-ai-workspaces.md`. Do not sign into a cloud provider or paste an API key if the intended baseline is local Ollama. Cline can run commands and edit files; use Plan mode first and keep approval prompts enabled. Do not enable auto-approve for the initial setup.

## 8. Verification checklist

```bash
ollama list
curl -fsS http://localhost:11434/api/tags
docker info --format '{{.ServerVersion}}'
cd ~/AI-Lab/infrastructure/docker && docker compose config && docker compose ps
uv python list --installed
git --version
code --version
```

Record actual outputs and versions. A missing or failing command is an open task, not a completed installation.

## 9. Operational cautions

- Docker Desktop and local models consume memory; avoid loading multiple large models simultaneously.
- Keep local services bound to localhost.
- Do not expose Ollama or Open WebUI publicly.
- Do not put secrets in shell history, source files, screenshots, or Git.
- A local Ollama model does not guarantee all extension traffic is local; review telemetry and integrations.
- This platform setup must remain agent-neutral. Do not add LinkedIn-specific prompts, data, API credentials, or agent runtime stores here.
