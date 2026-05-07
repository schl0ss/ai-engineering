---
name: anchor
description: Anchor parallel coding sessions, branches, worktrees, PRs, and production deploy state before updating production. Use when the user says "anchor" or asks an AI coding agent to reconcile drift across sessions, preserve unrelated work, move only related changes onto the real production deploy path, deploy an exact revision, and verify the live result. Works in local workspaces, cloud coding sandboxes, and CI-like environments by adapting evidence gathering to the access available.
---

# Anchor

Anchor repo reality before updating production: reconstruct durable state, classify drift, converge only related work, ship one exact revision, and verify the live result.

Treat the conversation as a lead, not proof. Git history, remotes, PRs, CI, deploy records, artifacts, images, and live production are evidence.

## Glossary

Use these terms exactly when reasoning and reporting. Consistent language is the point.

- **Workspace**: the checkout, local repo, cloud sandbox, or CI runner where the agent is operating.
- **Production path**: the branch, workflow, provider environment, artifact, image, release channel, or server checkout that actually updates production.
- **Durable source**: state that survives outside the current chat, such as git history, remotes, PRs, CI runs, deploy records, artifacts, images, issues, or live endpoints.
- **Drift**: any difference between the workspace, production path, parallel work, and live production.
- **Related drift**: drift that affects the same behavior, files, dependencies, deploy path, or verification target as the requested change.
- **Conflicting drift**: drift that overlaps the requested change but cannot be safely combined without human judgment.
- **Unrelated drift**: drift that does not affect the requested change. Leave it untouched.
- **Unknown drift**: drift that cannot yet be classified. Inspect further; if still unknown, stop.
- **Exact revision**: a commit SHA, image digest, build id, artifact hash, release id, deployment id, or server checkout SHA that can be named after shipping.
- **Anchor report**: the final record of production path, previous revision, shipped revision, drift handling, checks, live verification, tree status, and residual risk.

Key classifications:

- **Related**: include before shipping.
- **Conflicting**: stop and explain the collision.
- **Unrelated**: preserve and skip.
- **Unknown**: inspect, then stop if evidence is still insufficient.

## Principles

- **Evidence beats memory.** Reconstruct state from durable sources before acting.
- **Production path first.** Discover what actually updates production before editing.
- **Preserve unrelated work.** Do not reset, delete, stash, rewrite, or overwrite work from other sessions unless the user explicitly asks.
- **Converge narrowly.** Bring in only clearly related drift. Do not use broad sync commands to make drift disappear.
- **One exact revision ships.** Production should be updated by a named revision, artifact, image, build, release, or deployment.
- **Provider success is not verification.** Verify the live target independently.
- **Cloud is partial evidence.** In cloud sandboxes, assume the checkout is isolated. Hidden work on another machine cannot be reconciled unless surfaced through a worktree, branch, PR, patch, artifact, issue, or user-provided context.
- **Stop instead of guessing.** Stop before production mutation when production path, deployed revision, drift classification, credentials, or live verification cannot be established.

## Process

### 1. Explore project reality

Read project instructions before changing files:

- `AGENTS.md`
- `CLAUDE.md`
- `.cursor/rules/*`
- `docs/agents/*`
- contributing, deploy, release, or runbook docs

Extract the canonical commands, deploy policy, production branch or environment, and verification requirements.

Then inspect the strongest evidence the environment exposes:

- Local workspace: dirty files, local branches, local worktrees, remotes, PRs, CI, deploy records, and live production.
- Cloud sandbox: fetched branches, remotes, PRs, CI, deploy records, artifacts, images, and live production. Do not assume unpushed local work elsewhere is visible.
- CI-like runner: checked-out revision, workflow id, artifact, environment, forge state, provider state, and live production.

### 2. Map drift

Build a state map before editing. Include:

- Workspace path, branch, upstream, status, dirty files, untracked files, and unpushed commits.
- Accessible local worktrees with `git worktree list`, when available.
- Recent local and remote branches.
- Recent commits touching the requested files, routes, modules, services, assets, or dependencies.
- Open PRs and their branches.
- CI state for relevant branches and commits.
- Production path and latest deployed revision.
- Recent production deployments and their source revisions or artifacts.
- Live production URL or endpoint.

Cross-check at least two durable sources when possible. For example, compare provider deploy metadata with git history, or compare a live version endpoint with the claimed deployment revision.

### 3. Classify drift

Classify every material difference as **related**, **conflicting**, **unrelated**, or **unknown**.

Explain the classification in terms of behavior, files, dependencies, deploy path, or verification target. If related work is tangled with unrelated work, treat it as conflicting unless there is a clear narrow extraction.

If the production path is ambiguous, the deployed revision cannot be identified, required credentials are missing, or verification would require destructive production actions, stop and report the gap.

### 4. Converge related work

Use the repository's normal integration method: merge, rebase, cherry-pick, patch application, PR update, release branch update, or provider-specific promotion.

Prefer the method that preserves the clearest audit trail for this repo. Bring in only related drift. Preserve authorship and history when that matters to the project.

If related drift conflicts with the requested change, stop with the competing commits, branches, files, and risk.

### 5. Ship one exact revision

Make the requested change on the verified production path. Keep edits scoped to the requested behavior and any clearly related drift.

Before committing:

- Run the smallest useful checks first, then broader checks when the change touches shared behavior, deploy configuration, build output, dependencies, authentication, data, or user-facing flows.
- Inspect the staged diff.
- Confirm unrelated dirty files are not staged.
- Confirm the commit will land on the production path or the repo's required release path.

Commit only intended changes. Push through the repo's normal release process.

Deploy the exact revision through the real production mechanism: CI environment, hosting provider deploy, container image rollout, static asset publish, package release, or server pull-and-restart script.

Capture the strongest identifier available: deployment id, release id, build id, workflow run id, commit SHA, image digest, artifact hash, or server checkout SHA.

### 6. Verify live production

Check the live target, not just provider status.

Use evidence that proves the shipped change is present:

- Live HTML, JavaScript, CSS, static assets, API responses, headers, health checks, or version endpoints.
- Asset hashes, source maps, build ids, commit metadata, image digests, or artifact hashes.
- Browser, API, smoke, or command-line checks of the affected path.
- Cache behavior when CDN, static hosting, or split traffic is involved.

If live production does not match the exact revision, investigate wrong branch, stale cache, failed rollout, background deploy, or traffic splitting. Do not report success until live evidence matches.

### 7. Report

End with an anchor report:

- **Production path**: what actually updates production.
- **Previous revision**: what production was running before.
- **Shipped revision**: branch and exact revision shipped.
- **Deployment identifier**: deployment, release, build, artifact, image, workflow, or server identifier.
- **Drift handling**: related drift included, conflicting drift stopped on, unrelated drift skipped, unknown drift unresolved.
- **Checks**: validation run before deploy.
- **Live verification**: evidence from production.
- **Tree status**: final workspace state.
- **Risk**: residual risks or follow-up checks.
