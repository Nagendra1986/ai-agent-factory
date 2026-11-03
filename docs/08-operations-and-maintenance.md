# 08 — Operations, Backups, and Maintenance

## Local service operations

Open WebUI:
```bash
cd ~/AI-Lab/infrastructure/docker
docker compose ps
docker compose logs --tail=100 open-webui
docker compose restart open-webui
```

Ollama:
```bash
ollama list
curl -fsS http://localhost:11434/api/tags
```

Do not expose either service publicly by default.

## Backups

Back up:
- Open WebUI's Docker volume using a documented, tested backup procedure;
- agent SQLite databases;
- local configuration and prompt files that are not committed;
- private knowledge bases;
- governance and review records where needed.

Do not put backups in a public repository. Encrypt backups containing private or sensitive information. Test restoration periodically; an untested backup is not a recovery plan.

## Updates

- Review model updates before changing production agent defaults.
- Pin tested Docker images.
- Update Python dependencies through controlled changes.
- Run tests and security scans after updates.
- Record the previous known-good version and rollback procedure.

## Monitoring

Record:
- agent version;
- model identifier;
- run/correlation ID;
- duration and outcome;
- bounded retry count;
- tool calls and approval events;
- errors and recovery status.

Avoid logging full sensitive prompts, credentials, private messages, or personal data unless there is a justified and protected operational need.

## Incident response

If a credential is exposed:
1. revoke or rotate it immediately;
2. identify where it was used;
3. remove it from active configuration;
4. assess Git history and logs;
5. follow repository history cleanup guidance;
6. document the incident and preventive action.

If an agent performs an unintended external action:
1. stop further execution;
2. preserve relevant audit evidence;
3. assess whether the action can be reversed;
4. revoke or restrict affected tool access;
5. fix and test the failure mode before re-enabling.
