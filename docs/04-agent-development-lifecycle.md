# 04 — Standard Agent Development Lifecycle

Every agent follows this lifecycle. Scale the process to the risk and complexity of the application.

## Stage 1: Discover

Document:
- problem statement;
- intended users;
- business outcome;
- trigger and operating context;
- inputs and outputs;
- non-goals;
- external systems;
- constraints and assumptions.

Do not start coding while the purpose and boundaries remain ambiguous.

## Stage 2: Design

Produce:
- architecture diagram;
- component responsibilities;
- data-flow description;
- model selection rationale;
- tool and permission inventory;
- data classification;
- risk classification and threat model;
- state machine and recovery design;
- idempotency strategy;
- acceptance criteria.

Prefer a single-agent design unless multiple agents provide a clear benefit.

## Stage 3: Scaffold

Create an isolated project using the agent template:
- own `pyproject.toml` and `uv.lock`;
- own `.venv`;
- own configuration;
- own tests;
- own documentation;
- own runtime data location.

Do not copy secrets, user data, or another agent's database.

## Stage 4: Implement

Use small, reviewable changes. Separate:
- domain/business logic;
- model interaction;
- tools and integrations;
- persistence;
- UI/API;
- configuration;
- observability.

Validate all external inputs and model-generated structured outputs. Treat model output as untrusted.

## Stage 5: Verify

Run:
- formatting and linting;
- unit tests;
- integration tests;
- idempotency and retry tests;
- security scanners;
- dependency audit;
- prompt-injection and tool-abuse tests where relevant;
- AI code review.

Record actual command output or machine-readable reports.

## Stage 6: Pull request

- create a feature branch;
- commit only intended files;
- scan staged changes and Git history for secrets;
- push to GitHub;
- create a PR into `main`;
- let CI run;
- attach or link the local AI review report;
- inspect findings and resolve or document exceptions.

## Stage 7: Merge and release

The owner is the final decision-maker. No second human approval is required by this workflow.

Merge only when required checks pass and remaining findings are understood. Tag releases and maintain a changelog. For agents with external side effects, deploy first in a restricted or dry-run mode.

## Stage 8: Operate and improve

Define:
- logs and audit records;
- retention and backup;
- health checks;
- failure alerts;
- model/version tracking;
- rollback procedure;
- dependency update process;
- periodic security review.

## Definition of done

An agent is not done merely because it runs once. It must meet its acceptance criteria, have documented boundaries, pass applicable tests and scans, and have a recovery and maintenance approach.
