# 02 — One-time Mac Environment Setup

## Machine profile

Target machine: Apple M5 MacBook Pro, 24 GB unified memory, macOS 26.5 (owner-reported).

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
```

A missing command means that component may not yet be installed. Do not assume an installation succeeded without checking.

## 2. Install base tools

Install Homebrew from its official instructions if absent. Then:

```bash
brew update
brew install git uv jq
brew install --cask visual-studio-code
brew install --cask docker
```

Install Ollama using the official macOS installer: https://ollama.com/download

Open Docker Desktop once and complete its first-run setup.

## 3. Install Python

```bash
uv python install 3.12
uv python list --installed
```

Use `uv` to create a separate environment per agent. Do not install project dependencies into the system Python.

## 4. Install local models

Candidate starting models:

```bash
ollama pull llama3.1:8b
ollama pull qwen3:8b
ollama list
```

Start or open Ollama, then test a model:

```bash
ollama run llama3.1:8b
```

Exit the interactive session with `/bye`.

Do not assume the suggested model is optimal. Benchmark response quality, latency, memory use, structured output, and tool calling on representative tasks.

## 5. Create the workspace

```bash
mkdir -p ~/AI-Lab/{infrastructure/{docker,scripts,config},models,shared/{knowledge,prompts,schemas,utilities},agent-factory/{templates/{basic-agent,rag-agent,multi-agent},standards,documentation},agents,data/{databases,vector-store,backups},logs}
```

The `data`, `logs`, private knowledge, and local configuration directories must remain outside any public Git repository. Prefer storing runtime data under `~/AI-Lab/data/` or an agent-specific local data directory.

## 6. Open WebUI with Docker Compose

Create `~/AI-Lab/infrastructure/docker/.env` without overwriting an existing secret:

```bash
cd ~/AI-Lab/infrastructure/docker
umask 077
if [ ! -f .env ]; then
  printf 'WEBUI_SECRET_KEY=%s\n' "$(openssl rand -hex 32)" > .env
fi
```

Create `docker-compose.yml`:

```yaml
services:
  open-webui:
    image: ghcr.io/open-webui/open-webui:main
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

Start and verify:

```bash
docker compose up -d
docker compose ps
docker compose logs --tail=50 open-webui
```

Open http://localhost:3000 and create the initial administrator account.

After validating the installation, pin Open WebUI to a tested release tag or digest rather than leaving the mutable `main` tag in a long-lived setup. Back up its persistent volume before upgrades.

## 7. Install Roo Code

1. Open VS Code.
2. Open Extensions (`Cmd+Shift+X`).
3. Find and install Roo Code.
4. Configure its provider for Ollama using `http://localhost:11434`.
5. Select an installed model.
6. Keep terminal and file-operation approvals enabled.
7. Open the active project folder, not the entire home directory.

Follow the current Roo Code and Ollama integration documentation because settings and model support can change.

## 8. Verification

```bash
ollama list
curl -fsS http://localhost:11434/api/tags | jq '.models[].name'
docker info --format '{{.ServerVersion}}'
cd ~/AI-Lab/infrastructure/docker && docker compose ps
uv python list --installed
git --version
```

## 9. Operational cautions

- Docker Desktop and local models consume memory; avoid loading multiple large models simultaneously.
- Keep services bound to localhost.
- Do not expose Ollama or Open WebUI publicly.
- Do not put secrets in shell history or source files.
- Record the installed versions and model tags in a local environment inventory.
