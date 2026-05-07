# Handoff Skill

`handoff` aligns parallel coding sessions, branches, worktrees, PRs, and production deploy state before shipping.

Its core invariant is simple: reconstruct reality before acting, converge related work, ship the exact revision, and verify production.

## Install

Install from this repository with Codex's skill installer:

```bash
install-skill-from-github.py --repo schl0ss/handoff-skill --path handoff
```

Or install from a GitHub URL:

```bash
install-skill-from-github.py --url https://github.com/schl0ss/handoff-skill/tree/main/handoff
```

Restart Codex after installation so the skill is discovered.

## Use

Ask Codex:

```text
handoff
```

Or explicitly:

```text
Use $handoff to reconcile drift across sessions and ship this change to production.
```

The skill is platform-agnostic. It does not assume a specific deploy provider. It discovers the production deploy path from the repository, forge, CI, provider metadata, and live target.

## License

MIT
