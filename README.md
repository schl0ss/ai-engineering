# Anchor Skill

`anchor` aligns parallel coding sessions, branches, worktrees, PRs, and production deploy state before updating production.

Its core invariant is simple: reconstruct reality before acting, converge related work, ship the exact revision, and verify production.

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

The skill is platform-agnostic. It does not assume a specific deploy provider. It discovers the production deploy path from the repository, forge, CI, provider metadata, and live target.

## License

MIT
