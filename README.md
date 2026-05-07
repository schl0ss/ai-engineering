# Anchor Skill

`anchor` aligns parallel coding sessions, branches, worktrees, PRs, and production deploy state before updating production.

Its core invariant is simple: reconstruct reality before acting, converge related work, ship the exact revision, and verify production.

The skill is written as an agent playbook: explicit goal, terms, evidence rules, drift classifications, stop conditions, and final report shape. It is meant to be easy for an LLM to execute without relying on hidden assumptions.

## Compatibility

Anchor is packaged as an OpenAI/Agent Skills skill with `anchor/SKILL.md` and `anchor/agents/openai.yaml`.

The workflow itself is platform-agnostic. It is designed for:

- Local coding workspaces.
- Cloud coding sandboxes.
- CI-like runners.
- Any agent system that can read `SKILL.md` instructions.

In cloud environments, Anchor adapts to the evidence available there: remotes, PRs, CI records, deploy provider metadata, artifacts, images, and live production checks. It cannot see another machine's unpushed local work unless that work is exposed through a worktree, branch, PR, patch, artifact, issue, or user-provided context.

OpenAI skills follow the Agent Skills open standard and can be used across tools that support that format. See OpenAI's [Skills in ChatGPT](https://help.openai.com/articles/20001066-skills-in-chatgpt) and [Codex cloud](https://platform.openai.com/docs/codex) docs for current product support.

### Claude Code

Claude Code can use Anchor as a Markdown playbook, but it should not be treated as a native OpenAI skill install there. The portable part is `anchor/SKILL.md`; the `anchor/agents/openai.yaml` metadata is for tools that understand the OpenAI/Agent Skills package shape.

Recommended Claude Code usage:

- Paste or reference `anchor/SKILL.md` when asking Claude Code to reconcile drift before updating production.
- Create a custom slash command by placing the Anchor instructions in `.claude/commands/anchor.md` for a project command, or `~/.claude/commands/anchor.md` for a personal command, then run `/anchor`.
- For a dedicated workflow agent, adapt the same instructions into a Claude Code subagent under `.claude/agents/anchor.md`.

The same evidence rule applies in Claude Code: Anchor can only reconcile state Claude Code can inspect through the checkout, remotes, PRs, CI, deploy provider records, artifacts, or live production. Unpushed work on another machine remains invisible until it is surfaced through a durable source.

See Anthropic's Claude Code docs for [custom slash commands](https://docs.anthropic.com/en/docs/claude-code/slash-commands) and [subagents](https://docs.anthropic.com/en/docs/claude-code/sub-agents).

## Install

In Codex, ask:

```text
Install the anchor skill from https://github.com/schl0ss/anchor-skill/tree/main/anchor
```

If using the skill installer helper directly:

```bash
install-skill-from-github.py --repo schl0ss/anchor-skill --path anchor
```

Or install from a GitHub URL:

```bash
install-skill-from-github.py --url https://github.com/schl0ss/anchor-skill/tree/main/anchor
```

Restart Codex after installation so the skill is discovered.

## Use

Ask Codex:

```text
anchor
```

Or explicitly:

```text
Use $anchor to reconcile drift across sessions and update production.
```

The skill does not assume a specific deploy provider. It discovers the production deploy path from the repository, forge, CI, provider metadata, and live target.

## License

MIT
