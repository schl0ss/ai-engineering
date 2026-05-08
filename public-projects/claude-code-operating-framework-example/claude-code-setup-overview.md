# Claude Code Setup Overview

Public artifact for reviewer use.

Repository: https://github.com/schl0ss/Claude-Code-Operating-Framework-Example

This is a sanitized Claude Code operating framework built around a fictional service-risk triage scenario for generic logistics operations. It is not a client system, a copied production workflow, or a turnkey accelerator. It preserves the engineering shape of the setup while removing client data, proprietary prompts, credentials, routing heuristics, production orchestration, and reusable private machinery.

## What This Setup Demonstrates

- Sub-agent topology for intake, planning, data, decisioning, critique, and safety review.
- Claude Code project artifacts, including `.claude/agents`, `.claude/skills`, `.claude/settings.json`, hooks, and `.mcp.json`.
- Durable context through `CONTEXT.md`, role contracts, workflow state, artifacts, risks, and approval records.
- Tool boundaries that keep agent roles scoped instead of giving every role unrestricted access.
- Human approval gates before customer-facing, production-impacting, financial, or restricted-data actions.
- Evaluation through rubrics, regression checks, and explicit review criteria.

## Fictional Demo Scenario

The demo case is service risk triage for generic logistics operations.

Given synthetic shipment events, asset snapshots, maintenance notes, and route constraints, the framework produces a reviewable operational risk brief and proposed next actions. Anything that would affect customers, production systems, spending, or restricted data stops at an approval gate.

This scenario is specific enough to show data engineering, analytics, and decisioning fluency. It is generic enough not to expose anyone else's intellectual property.

## Claude Code Artifacts

- `.claude/agents`: public role cards for specialized agents.
- `.claude/skills`: task-specific skills for service-risk triage and evaluation gating.
- `.claude/hooks`: illustrative hook wiring for project controls.
- `.claude/settings.json`: shared project behavior and hook configuration.
- `.mcp.json`: project-scoped toy MCP server configuration.
- `CONTEXT.md`: domain language and resolved design decisions.
- `framework/agent-contract.yaml`: public contract for roles, gates, and artifacts.
- `framework/workflows/service-risk-triage.md`: workflow playbook for the synthetic scenario.
- `docs/guardrails.md`: approval gates, data policy, and tool constraints.
- `docs/evaluation.md` and `evals/rubric.md`: scoring model and review process.

## Operating Loop

1. Intake captures the user request, goal, constraints, expected artifacts, and risk profile.
2. Planning turns the request into bounded work with named deliverables and approval requirements.
3. Data review inspects allowed sources and produces evidence-backed findings.
4. Decisioning drafts recommended actions with assumptions and unresolved risks.
5. Critique reviews reasoning quality, missing evidence, and failure modes.
6. Safety checks whether outputs need human approval or must be blocked.
7. Evaluation scores the result against rubrics rather than relying on vibes.

Each step produces an inspectable artifact. The setup is designed so an agent can be useful without becoming opaque.

## Guardrail Model

The framework treats autonomy as something to be granted deliberately, not assumed globally.

- Agents get scoped roles and tool access.
- Customer-facing communication requires approval.
- Production writes require approval.
- Financial or contractual actions require approval.
- Restricted-data access is blocked in the public setup.
- Synthetic demo data is used for public review.
- Evaluation artifacts make failures visible instead of burying them in chat transcripts.

## What Is Intentionally Redacted

- Proprietary prompts.
- Private routing heuristics.
- Client workflows.
- Vendor credentials.
- Production eval sets.
- Real Databricks workspace configuration.
- Reusable pipeline or MCP connector machinery.
- Anything that would expose someone else's IP.

The public repo proves composition, control design, and engineering judgment. It does not publish the private machinery that would make the system production-specific.

## Fast Reviewer Path

1. Read the repository README.
2. Open `framework/agent-contract.yaml`.
3. Skim one file in `.claude/agents`.
4. Skim one file in `.claude/skills`.
5. Read `docs/guardrails.md`.
6. Read `docs/evaluation.md` or `evals/rubric.md`.
7. Run `npm run demo` if you want the synthetic trace.

## Why This Is The Right Artifact

The employer asked for a GitHub link to a PDF showing a Claude Code setup that has been generalized or fictionalized. This artifact packages the setup for that review path: one PDF, backed by a public repo, with the Claude Code structure preserved and sensitive implementation details removed.
