# 09 — Implementation Roadmap: Platform First, Agents Second

This is a sequenced delivery plan. A checkbox is not complete until the exit evidence exists. **Do not start an agent build as a substitute for completing the platform.**

# Workstream A — Reusable Agent Factory platform

## Phase A1 — Mac foundation

- [ ] Inspect existing Mac installations and record versions.
- [ ] Verify Homebrew, Git, uv, Docker Desktop, VS Code, and Ollama.
- [ ] Install only missing base tools.
- [ ] Install one candidate local model and verify inference.
- [ ] Create the local/private data root outside the public repository.

Exit: base tools and local model API work; versions are recorded; no private runtime data is in Git.

## Phase A2 — Platform workspaces

- [ ] Configure Open WebUI as a general-purpose local chat/design workspace.
- [ ] Verify Open WebUI can reach local Ollama.
- [ ] Verify persistent storage and local-only binding.
- [ ] Use the official Cline extension (publisher: Cline) in VS Code.
- [ ] Configure Cline to use local Ollama; keep approvals enabled and auto-approve disabled.
- [ ] Add reusable, agent-neutral development instructions.

Exit: a generic design discussion and a harmless sample coding task work without any agent-specific setup.

## Phase A3 — Factory repository and standards

- [ ] Establish repository structure: docs, standards, templates, infrastructure, and workflows.
- [ ] Add platform/agent boundary and public-repository data policy.
- [ ] Add reusable agent template and governance manifest.
- [ ] Add `.gitignore` and verify nested `.env`, data, logs, and local state are ignored.
- [ ] Review current Git history and staged files for secrets/private data.

Exit: sanitised factory source and documentation are version-controlled; runtime data is external to the repo.

## Phase A4 — Deterministic verification and security

- [ ] Configure Ruff and pytest conventions.
- [ ] Configure Bandit, Gitleaks, pip-audit, and Trivy as appropriate.
- [ ] Create local scripts with safe, repeatable behaviour.
- [ ] Test scanners against safe, controlled sample findings.
- [ ] Document findings triage and exception handling.

Exit: each selected tool runs and reports expected results; no scanner is described as passing without evidence.

## Phase A5 — Local AI peer review

- [ ] Define a read-only Git diff input.
- [ ] Define structured finding schema (severity, file/line, evidence, confidence, remediation).
- [ ] Run a local review model; optionally run a second model sequentially.
- [ ] Consolidate findings and generate a review report.
- [ ] Test with intentionally seeded defects.

Exit: review is repeatable, traceable, read-only, and explicitly advisory.

## Phase A6 — GitHub and CI

- [ ] Configure branch protection/rulesets for `main`.
- [ ] Add least-privilege GitHub Actions checks.
- [ ] Pin third-party Actions to verified full commit SHAs before mature use.
- [ ] Require deterministic checks; do not require a second human approver.
- [ ] Test a PR with both passing and failing checks.
- [ ] Verify no secrets are exposed to untrusted PR code.

Exit: mandatory CI gates behave as intended; owner retains merge authority.

## Phase A7 — Platform acceptance and operational readiness

- [ ] Complete `docs/platform-acceptance-checklist.md`.
- [ ] Verify backups and perform a restore test for persistent platform data.
- [ ] Document upgrade and rollback procedures.
- [ ] Record unresolved risks and explicitly accept or remediate them.

Exit: the platform is accepted as a reusable foundation. No specific agent is required to satisfy this phase.

# Workstream B — Individual agents (only when needed)

## Phase B1 — Select and approve a use case

- [ ] Write a problem statement and measurable outcome.
- [ ] Confirm scope, non-goals, data, integrations, risk, and approval boundaries.
- [ ] Decide whether the use case warrants an agent at all.

## Phase B2 — Build the agent as a separate workload

- [ ] Copy the agent template into its own project/repository.
- [ ] Complete its governance manifest and threat model.
- [ ] Choose only the dependencies and infrastructure it needs.
- [ ] Keep its data, secrets, databases, and logs local/private.
- [ ] Implement, test, scan, review, PR, release, and operate using the standard lifecycle.

## Candidate example: LinkedIn Response Agent

This is a possible first consumer of the platform, not part of the platform prerequisite. If selected later, it should accept post content supplied manually, use an approved private knowledge base, draft a response, and require manual review/publication. Do not assume scraping or automated posting is in scope.

## Completion principle

The factory is complete when its own acceptance criteria are met. An agent is complete only when that individual agent meets its separate acceptance criteria.
