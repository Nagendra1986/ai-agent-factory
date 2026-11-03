# 07 — GitHub, Pull Requests, and CI

## Repository strategy

Recommended:
- one public `ai-agent-factory` repository for reusable, intentionally public standards, templates, generic code, and documentation;
- separate repositories for agents whose source is suitable for publication;
- private or local-only storage for personal knowledge, private datasets, credentials, databases, logs, and generated content.

Do not assume every agent must be public. Public source code can reveal prompts, architecture, endpoints, or operational details. Review before publishing.

## Branching

Use a simple GitHub Flow:

```text
main
  |
  +-- feature/short-description
          |
          +-- implementation
          +-- local tests and scans
          +-- local AI review
          +-- push branch
          +-- pull request to main
          +-- GitHub Actions checks
          +-- owner reviews evidence
          +-- merge
          +-- tag/release when appropriate
```

## Pull request policy

- PR required before merging into `main`.
- No second human approval required.
- Required automated status checks must pass.
- Resolve PR conversations before merging.
- Disable force pushes and branch deletion on `main`.
- The owner reviews the diff, CI output, AI review report, and any waivers.
- No AI agent may auto-approve or auto-merge.

For public repositories on GitHub Free, branch protection and required status checks are available. Configure required approving reviews to zero if the interface permits; the workflow does not require a second human approver. Verify current plan behaviour in GitHub settings.

## GitHub Actions security

- Set minimal workflow permissions, normally `contents: read`.
- Pin third-party Actions to verified full commit SHAs before relying on them.
- Do not expose secrets to untrusted pull-request code.
- Do not use `pull_request_target` to check out and execute untrusted PR code.
- Avoid privileged self-hosted runners for public PRs.
- Keep AI review local unless a separately secured service is designed.
- Use dependency pinning and lockfiles.

## Example CI workflow

This is a starter example. Before production use, pin each third-party Action to a verified full commit SHA and confirm the project has the referenced tools and lockfile.

```yaml
name: Quality and Security

on:
  pull_request:
    branches: [main]
  push:
    branches: [main]

permissions:
  contents: read

jobs:
  python-checks:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: astral-sh/setup-uv@v5

      - run: uv sync --locked --all-groups

      - run: uv run ruff check .
      - run: uv run ruff format --check .
      - run: uv run bandit -r src
      - run: uv run pytest -v
      - run: uv run pip-audit

  secret-scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - uses: gitleaks/gitleaks-action@v2
```

The example is not a complete universal workflow. Adjust paths, dependency groups, action versions, and scanning configuration to the project.

## Initial Git hygiene

Before first push:

```bash
git status --short
git check-ignore -v .env data/agent.db
gitleaks detect --source . --redact
```

If a secret was committed, removing the file in a later commit is not enough. Revoke/rotate the secret and follow GitHub's history-remediation guidance.

## PR description template

```markdown
## Purpose
What problem does this change solve?

## Changes
What was changed?

## Tests
Which commands were executed and what were the results?

## Security
Which scans were run? Any findings?

## AI review
Reviewer model(s), report location, and unresolved observations.

## Governance
Any changes to data, tools, permissions, or external actions?

## Idempotency and recovery
What happens on retry, duplicate execution, or interruption?

## Risk / waiver
List any accepted risks and rationale.
```
