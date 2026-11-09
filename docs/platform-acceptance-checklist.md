# Platform Acceptance Checklist

Use this checklist to decide whether the reusable Agent Factory foundation is ready to support future agent projects. This checklist is agent-neutral: it does not require a LinkedIn account, LinkedIn data, an agent prompt, or an agent-specific database.

## 1. Local foundation

- [ ] Mac and tool versions recorded.
- [ ] Git and uv work.
- [ ] Docker Desktop works, if Docker services are used.
- [ ] Ollama is running locally and a selected model responds.
- [ ] Model memory/latency are acceptable for the intended development tasks.

Evidence: version inventory, model tag, successful local API/inference test.

## 2. Developer workspaces

- [ ] Open WebUI is available on localhost and can reach Ollama.
- [ ] Open WebUI persistent data location is known and backed up.
- [ ] Official Cline extension is installed from publisher Cline.
- [ ] Cline uses local Ollama for local-only work.
- [ ] Approval prompts remain enabled; auto-approve is disabled.
- [ ] Cline workspace is limited to the intended repository.

Evidence: screenshots/settings with secrets redacted, successful generic sample task, reviewed diff.

## 3. Repository and privacy boundary

- [ ] Public repository contains only intentionally public code/docs/templates.
- [ ] `.env`, credentials, databases, logs, chat history, private knowledge, and generated user data are outside Git.
- [ ] `git check-ignore` confirms expected ignore rules.
- [ ] Gitleaks scans the working tree and relevant Git history.
- [ ] Any previously committed secret has been revoked/rotated and history remediation completed.

Evidence: Git status, ignore checks, scanner report, repository review.

## 4. Quality and security controls

- [ ] Lint/format and test conventions documented.
- [ ] Relevant scanners execute and results are reviewed.
- [ ] Local AI review is read-only and advisory.
- [ ] AI findings include evidence and do not auto-approve or merge.
- [ ] Third-party tools/actions and MCP integrations are reviewed before use.

Evidence: test and scan reports, AI review sample, tool inventory.

## 5. GitHub workflow

- [ ] PR workflow into `main` is documented.
- [ ] Required CI checks work on a test PR.
- [ ] Least-privilege permissions are set.
- [ ] Third-party Actions are pinned to verified full SHAs before mature adoption.
- [ ] No privileged self-hosted Mac runner executes untrusted public PR code.
- [ ] Owner manually reviews and merges.

Evidence: test PR URL, successful/failed check examples, ruleset configuration.

## 6. Operations and recovery

- [ ] Persistent Docker data and local configuration locations are documented.
- [ ] Backups are protected and outside public GitHub.
- [ ] Restore has been tested.
- [ ] Upgrade and rollback procedures exist.

Evidence: backup and restore record, maintenance notes.

## Acceptance decision

- [ ] All mandatory items completed.
- [ ] Remaining gaps and risks documented.
- [ ] Platform owner explicitly accepts the foundation for future agent projects.

Only after this decision should an individual agent project be started using the standard lifecycle.
