# 05 — Agent Governance and Risk

## Objective

Govern not only the source code but also the agent's behaviour, data access, tool permissions, and external actions.

A clean static-analysis report does not prove that an agent is safe. Governance must be designed into the workflow.

## Risk classification

Use impact and autonomy to determine scrutiny. The examples below are indicative; classify each agent based on its actual capabilities.

| Risk | Example | Minimum controls |
|---|---|---|
| Low | Local summarisation of non-sensitive text | Input validation, tests, logging |
| Medium | Research agent retrieving external documents | Source validation, prompt-injection tests, bounded tools |
| High | Agent preparing or sending messages | Explicit send approval, recipient validation, audit |
| Critical | Financial transactions or infrastructure changes | Strong authorisation, transaction limits, independent controls, rollback and explicit approval |

## Mandatory governance questions

### Purpose and boundaries
- What is the agent allowed to do?
- What is explicitly out of scope?
- What constitutes success or failure?
- What actions require human approval?

### Identity and permissions
- Does the agent run with least privilege?
- Are tools allowlisted?
- Is filesystem access limited to the project/data directories?
- Are network destinations restricted where practical?
- Can the agent execute shell commands? If yes, which commands and under what approval?

### Data governance
- What data is collected and processed?
- Is it personal, confidential, regulated, or proprietary?
- Where is it stored?
- What is the retention and deletion process?
- Are logs minimised and redacted?
- Is any data sent to an external service?

### Model governance
- Which model and version are used?
- What are known limitations?
- How are hallucinations and unsupported claims handled?
- Are model outputs validated against schemas?
- Can retrieved content influence tool instructions?

### Human oversight
Require explicit approval for sensitive or irreversible actions such as publishing, sending messages, deleting data, modifying external systems, or making consequential decisions.

### Reliability
- Are retries bounded?
- Are operations idempotent?
- Is there a timeout?
- Can work resume after interruption?
- Are partial failures visible?
- Is there a rollback or compensation path?

### Auditability
Record enough to reconstruct important actions:
- correlation/run ID;
- agent and version;
- model identifier;
- input reference or safe digest;
- tool invoked;
- decision and validation result;
- approval event;
- outcome and timestamp.

Do not log secrets or unnecessary sensitive content.

## Example governance manifest

Each agent should maintain a reviewed `agent-governance.yaml`. Tailor values to the application; this is a starting schema, not a universal risk rating.

```yaml
agent:
  name: example-agent
  version: 0.1.0
  owner: local-user

risk:
  classification: medium
  rationale: "Explain expected impact and autonomy."

permissions:
  filesystem:
    allowed_paths: []
  network:
    allowed_destinations: []
  tools: []
  shell_access: false

human_oversight:
  required_for:
    - external_publication
    - sending_messages
    - deleting_data
    - modifying_external_systems

data:
  classification: internal
  local_storage_preferred: true
  retention_policy: required
  sensitive_data_logging: false

reliability:
  idempotency_required: true
  bounded_retries: true
  recovery_required: true
  audit_logging: true

security:
  validate_external_inputs: true
  validate_model_outputs: true
  prompt_injection_testing: true

release:
  automated_checks_required: true
  local_ai_review_required: true
  owner_decision_required: true
```

## Illustrative future workload: LinkedIn Response Agent

If this candidate is selected after platform acceptance, the agent would draft responses. This use case is illustrative and is not part of platform prerequisites. It should not scrape LinkedIn, automate browser interactions, or publish comments. The user supplies post text and URL, reviews the draft, and publishes manually. Store private knowledge and response history locally, never in a public repository.

## Governance references

Use current versions of:
- NIST AI Risk Management Framework;
- OWASP Top 10 for LLM Applications;
- OWASP guidance for agentic applications;
- applicable privacy, platform, and organisational requirements.

This document is an engineering baseline, not legal or regulatory certification.
