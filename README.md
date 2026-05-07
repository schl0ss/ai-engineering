# AI Engineering

Public proof-of-work for AI engineering: reusable agent skills, interview-safe project examples, and a clean boundary between public artifacts and private operating context.

The goal is not to collect prompts. The goal is to show systems judgment: agent workflows that survive drift, projects with inspectable architecture, and private context handled with restraint.

## Repository Map

```text
skills/
  engineering/
    anchor/                 Production-readiness skill for converging drift before shipping

public-projects/
  claude-code-operating-framework-example/
                            Project card linking to the standalone public framework example
  georgia-tech-data-science-portfolio/
                            Placeholder for public data science portfolio artifacts

private-projects/
  README.md                 Explains what belongs in local-only project context
  .gitignore                Keeps private project contents out of git

docs/
  adr/                      Decisions about this repository

scripts/                    Helper scripts for repo maintenance
```

## What Belongs Here

- `skills/`: reusable agent workflows and operating procedures. These should be portable across Claude Code, Codex, and other coding agents that can read Markdown instructions.
- `public-projects/`: sanitized, interview-safe proof-of-work. Public projects prove the shape of the work without publishing private machinery, credentials, client context, or production details.
- `private-projects/`: local-only context for real projects. This includes private prompts, client notes, deploy paths, production URLs, internal eval sets, credentials references, and anything that should not be published.
- `docs/adr/`: decisions about this portfolio repo itself.

Public projects prove the shape of the work. Private projects hold the sensitive context that should not be published.

## Featured Work

### Anchor

`skills/engineering/anchor` is a vendor-neutral agent skill for updating production after parallel AI coding sessions. It reconstructs repo reality from durable sources, classifies drift, converges only related work, ships one exact revision, and verifies the live result.

Install path:

```text
https://github.com/schl0ss/ai-engineering/tree/main/skills/engineering/anchor
```

### Claude Code Operating Framework Example

`public-projects/claude-code-operating-framework-example` points to a standalone public example of an agentic engineering operating framework: sub-agents, skills, hooks, MCP, guardrails, synthetic data, approval gates, and evaluation.

Project URL:

```text
https://github.com/schl0ss/Claude-Code-Operating-Framework-Example
```

### Georgia Tech Data Science Portfolio

`public-projects/georgia-tech-data-science-portfolio` is reserved for public portfolio artifacts from Georgia Tech data science work: case studies, notebooks, reports, modeling decisions, and project context.

## Compatibility

The canonical skill artifact is `SKILL.md`. No product-specific file defines behavior.

Claude Code, Codex, and other coding agents should all read the same instructions. Product-specific adapters are optional and should not create divergent behavior.

## License

MIT licensed. See [LICENSE](LICENSE).
