# 04 — Standard Agent Development Lifecycle

This lifecycle applies to **each future agent project** after the Agent Factory platform has passed its own acceptance checklist. Platform installation is not an agent-development stage, and no agent is assumed to be in scope by default.

## Stage 0: platform readiness

Confirm that the factory platform acceptance criteria are met: local model inference, chat/coding workspaces, repository hygiene, tests/scanners, local AI review, GitHub PR/CI, and backup/recovery evidence. If the platform is not ready, return to the platform roadmap rather than embedding workarounds into an agent.

## Stage 1: discover

Document the problem, users, expected outcome, trigger, operating context, inputs/outputs, non-goals, external systems, constraints, and assumptions. Do not code while purpose and boundaries remain ambiguous.

## Stage 2: design

Produce the architecture, component responsibilities, data flow, model rationale, tool/permission inventory, data classification, risk/threat model, state/recovery design, idempotency strategy, acceptance criteria, and operational plan. Prefer a single-agent design unless multiple agents provide a clear benefit.

## Stage 3: scaffold

Create an isolated project from `templates/agent-template/` with its own `pyproject.toml`, `uv.lock`, `.venv`, configuration, tests, documentation, governance manifest, and local runtime-data directory. Do not copy secrets, user data, or another agent's database.

## Stage 4: implement

Use small, reviewable changes. Separate domain logic, model interaction, tools/integrations, persistence, UI/API, configuration, and observability. Validate external inputs and model-generated structured outputs. Treat model output and retrieved content as untrusted.

## Stage 5: verify

Run relevant formatting/linting, unit/integration/idempotency tests, security scanners, dependency audits, prompt-injection/tool-abuse tests, and local AI review. Record actual outputs and unresolved findings.

## Stage 6: pull request

Create a feature branch, commit only intended files, scan staged changes and Git history for secrets, push, open a PR into `main`, run CI, attach/link local AI review evidence, and resolve or document findings.

## Stage 7: merge and release

The owner is the final decision-maker; no second human approver is required by this workflow. Merge only when mandatory checks pass and remaining risks are understood. Tag releases and maintain a changelog. Deploy in dry-run or restricted mode when the agent can create external side effects.

## Stage 8: operate and improve

Define logs/audit records, retention, backup, health checks, failure alerts, model/version tracking, rollback, dependency updates, and periodic security review.

## Definition of done

An agent is not done merely because it runs once. It must meet acceptance criteria, have documented boundaries, pass applicable tests/scans, and have a recovery and maintenance approach.
