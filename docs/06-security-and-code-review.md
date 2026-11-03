# 06 — Security Verification and AI Code Review

## Review philosophy

The code-generating model must not be the only reviewer. Use deterministic tools, tests, a separate local AI review pass, and the owner's final decision.

AI review is advisory. It can miss vulnerabilities, invent issues, or share blind spots with the coding model. It is not a security guarantee or a substitute for specialist review on high-impact systems.

## Recommended free/open-source tools

| Tool | Purpose |
|---|---|
| Ruff | Python linting and formatting |
| Bandit | Common Python security issue detection |
| Gitleaks | Secret detection in files and Git history |
| pip-audit | Known vulnerabilities in Python dependencies |
| Trivy | Filesystem, dependency, container and configuration scanning |
| pytest | Unit and integration tests |

Check current installation and licence details from each project's official documentation before standardising versions.

## Local AI review

Use a different model or model configuration from the one that generated the change where practical. Review sequentially on a 24 GB machine to manage memory.

Inputs to the reviewer:
- original requirements;
- architecture and threat model;
- governance manifest;
- Git diff;
- relevant source and tests;
- test and scanner results.

Review for:
1. security flaws and unsafe defaults;
2. broken authorisation or excessive tool permissions;
3. prompt injection and untrusted content handling;
4. secrets and sensitive-data exposure;
5. incorrect business logic;
6. race conditions and unsafe state transitions;
7. idempotency and retry behaviour;
8. error handling and recovery;
9. missing tests;
10. maintainability and unnecessary complexity.

The reviewer should be read-only. It must not edit, approve, merge, or publish code.

## Structured finding format

Each finding should contain:
- stable finding ID;
- severity: critical, high, medium, low, or informational;
- category;
- file and line/range;
- evidence and exploit/failure scenario;
- confidence;
- recommendation;
- test needed to validate remediation.

Avoid reporting a vague concern as a confirmed vulnerability. Distinguish evidence from speculation.

## Review outcome

- BLOCK: critical/high verified issue or mandatory test/security gate failure.
- NEEDS REVIEW: medium issue, uncertain finding, or missing evidence.
- PASS WITH NOTES: no blocking finding; low/informational observations remain.
- WAIVER: owner records the accepted risk, rationale, scope, and date.

Do not let the AI reviewer silently downgrade or waive its own findings.

## Suggested local command sequence

Commands depend on the project's dependency groups and configuration. Run from the agent repository:

```bash
uv run ruff check .
uv run ruff format --check .
uv run bandit -r src
uv run pytest -v
uv run pip-audit
gitleaks detect --source . --redact
trivy fs --scanners vuln,misconfig,secret .
```

Adjust `src` and scanner scope to the actual project. Avoid scanning virtual environments and generated data unnecessarily. Run Gitleaks against Git history as well as the working tree.

## Local review versus GitHub CI

GitHub-hosted runners cannot reach the Mac's localhost Ollama service. Therefore:
- run local AI review on the Mac before pushing or creating the PR;
- upload a sanitised review report or attach it to the PR;
- use GitHub Actions for deterministic CI checks;
- do not expose Ollama to the internet;
- do not run untrusted public PR code on a self-hosted Mac runner.

## Reviewer independence

Two local models provide different perspectives, not independent assurance. Static scanners, test cases, threat modelling, and human judgement remain important. For critical systems, obtain qualified independent security review before real-world use.
