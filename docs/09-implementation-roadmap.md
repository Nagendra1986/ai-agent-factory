# 09 — Implementation Roadmap

This roadmap is a sequence, not evidence that the work is already installed.

## Phase 1 — Mac foundation

- [ ] Verify Homebrew, Git, uv, Docker Desktop, and Ollama.
- [ ] Install Python 3.12 through uv.
- [ ] Download candidate local models.
- [ ] Verify local model inference.
- [ ] Record versions and model tags.

Exit criteria: all required commands work and local inference succeeds.

## Phase 2 — User interfaces

- [ ] Start Open WebUI with persistent Docker storage.
- [ ] Verify Ollama connectivity from Open WebUI.
- [ ] Install VS Code and Roo Code.
- [ ] Connect Roo Code to local Ollama.
- [ ] Enable approval controls.
- [ ] Add global and repository-level development rules.

Exit criteria: chat and project-scoped coding workflows work.

## Phase 3 — Agent Factory repository

- [ ] Create the directory structure.
- [ ] Add standards and documentation.
- [ ] Add agent templates.
- [ ] Add `.gitignore` and public repository policy.
- [ ] Initialise Git.
- [ ] Review all files for private information before making the repository public.

Exit criteria: a clean, sanitised repository is ready for its first push.

## Phase 4 — Verification toolkit

- [ ] Install Ruff.
- [ ] Install Bandit.
- [ ] Install Gitleaks.
- [ ] Install pip-audit.
- [ ] Install Trivy.
- [ ] Add pytest conventions.
- [ ] Create a local review script.
- [ ] Test scanners against safe sample findings.

Exit criteria: each scanner executes and reports results correctly.

## Phase 5 — AI peer review

- [ ] Define reviewer input and output schema.
- [ ] Implement read-only Git diff collection.
- [ ] Run first review model.
- [ ] Run second review model sequentially.
- [ ] Consolidate and deduplicate findings.
- [ ] Generate a review report.
- [ ] Test with intentionally seeded code defects.

Exit criteria: review output is structured, traceable, and clearly advisory.

## Phase 6 — GitHub and CI

- [ ] Create public Agent Factory repository.
- [ ] Push sanitised source.
- [ ] Add GitHub Actions workflow.
- [ ] Require CI checks on main.
- [ ] Configure PR policy without second-human approval.
- [ ] Test a PR with passing and failing checks.
- [ ] Document release and rollback process.

Exit criteria: a PR cannot merge while mandatory CI checks fail.

## Phase 7 — Factory acceptance test

Build a small sample agent using only the documented factory process.

Validate:
- project scaffolding;
- isolated dependencies;
- idempotent setup;
- unit and integration tests;
- security scans;
- AI review;
- PR and CI;
- local deployment;
- backup and recovery.

## Phase 8 — Agent #001: LinkedIn Response Agent

Only after the factory acceptance test:
- define its scope and governance manifest;
- implement manual post input;
- classify relevance;
- retrieve local knowledge;
- draft a response;
- prevent duplicate processing;
- preserve versions and audit events;
- require manual approval and publication;
- keep private data out of public GitHub.

## Completion principle

Do not mark a phase complete because files were created. Mark it complete only after the stated exit criteria have been tested on the Mac.
