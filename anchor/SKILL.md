---
name: anchor
description: Anchor parallel coding sessions, branches, worktrees, PRs, and production deploy state before updating production. Use when the user says "anchor" or asks an AI coding agent to reconcile drift across sessions, preserve unrelated work, move only related changes onto the real production deploy path, deploy an exact revision, and verify the live result. Works in local workspaces, cloud coding sandboxes, and CI-like environments by adapting evidence gathering to the access available.
---

# Anchor

## Goal

Before updating production, reconstruct the repo's real state, reconcile related drift, ship one exact revision, and verify the live result.

Treat the conversation as a lead. Treat git, remotes, PRs, CI, deploy metadata, artifacts, and live production as evidence.

## Core Rule

Do not update production until these questions have evidence-backed answers:

1. What source does production actually deploy from?
2. What revision is production running now?
3. What parallel work exists in this repo or forge?
4. Which drift is related to this change?
5. What exact revision will be deployed?
6. How will the live result be verified?

If any answer is unknowable from available evidence, stop and report the gap.

## Terms

- `workspace`: the checkout or sandbox where the agent is running.
- `production path`: the branch, workflow, provider, environment, artifact, image, or server checkout that actually updates production.
- `durable source`: state that survives outside the current chat, such as git history, remotes, PRs, CI runs, deploy records, artifacts, images, issues, or live endpoints.
- `drift`: any difference between the workspace, production path, parallel work, and live production.
- `exact revision`: a commit SHA, image digest, build id, artifact hash, release id, or deployment id that can be named after shipping.
- `anchor report`: the final record of sources checked, drift handled, revision shipped, live verification, and remaining risk.

## Environment Rule

Use the strongest evidence the environment exposes:

- Local workspace: inspect dirty files, local branches, local worktrees, remotes, PRs, CI, deploy metadata, and live production.
- Cloud sandbox: assume the checkout is isolated. Inspect fetched branches, remotes, PRs, CI, deploy metadata, artifacts, images, and live production. Do not assume another machine's unpushed work is visible.
- CI-like runner: treat the runner as evidence for its checked-out revision, workflow id, artifact, and environment. Use forge and provider APIs for broader state.

Hidden work cannot be reconciled unless it is exposed through a worktree, branch, PR, patch, artifact, issue, or user-provided context.

## Non-Negotiables

- Preserve unrelated work. Do not reset, delete, stash, rewrite, or overwrite work from other sessions unless the user explicitly asks.
- Discover the production path before editing.
- Work on the production path, or move only clearly related changes onto that path.
- Deploy one exact revision.
- Verify live production independently of provider success.
- Report skipped drift with reasons.

## Procedure

### 1. Read Project Instructions

Before changing files, read available project rules:

- `AGENTS.md`
- `CLAUDE.md`
- `.cursor/rules/*`
- `docs/agents/*`
- contributing, deploy, release, or runbook docs

Extract the canonical commands, deploy policy, production branch or environment, and verification requirements.

### 2. Map Durable State

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

Cross-check at least two durable sources when possible. Example: compare provider deploy metadata with git history, or compare a live version endpoint with the claimed deployment revision.

### 3. Classify Drift

Classify each difference found in the state map:

- `related`: affects the same behavior, files, dependencies, deploy path, or verification target. Include it before shipping.
- `conflicting`: overlaps the change but cannot be safely combined without human judgment. Stop.
- `unrelated`: leave it untouched.
- `unknown`: inspect further; if still unknown, stop.

Do not use broad sync commands to make drift disappear. Explain what each relevant difference is and why it belongs in one class.

### 4. Choose The Convergence Plan

State the plan before making material changes when drift exists.

Use the repository's normal integration method: merge, rebase, cherry-pick, patch application, PR update, release branch update, or provider-specific promotion. Prefer the method that preserves the clearest audit trail for this repo.

If related work is missing from the production path, bring in only that work. If the related work is too tangled with unrelated work, stop and report the boundary.

### 5. Make The Change

Make the requested change on the verified production path. Keep edits scoped to the requested behavior and any clearly related drift.

Before committing, run the smallest useful checks first. Run broader checks when the change touches shared behavior, deploy configuration, build output, dependencies, authentication, data, or user-facing flows.

### 6. Commit And Publish

Commit only intended changes. Before committing:

- Inspect the staged diff.
- Confirm unrelated dirty files are not staged.
- Confirm the commit will land on the production path or the repo's required release path.

After committing, record the branch, exact commit SHA, and tree status. Push through the repo's normal release process.

### 7. Deploy One Exact Revision

Deploy the exact revision through the real production mechanism. Examples include CI environments, hosting provider deploys, container image rollouts, static asset publishing, package releases, or server pull-and-restart scripts.

Capture the strongest identifier available:

- Deployment id.
- Release id.
- Build id.
- Workflow run id.
- Commit SHA.
- Image digest.
- Artifact hash.
- Server checkout SHA.

### 8. Verify Live Production

Provider success is not verification. Check the live target.

Use evidence that proves the shipped change is present:

- Live HTML, JavaScript, CSS, static assets, API responses, headers, health checks, or version endpoints.
- Asset hashes, source maps, build ids, commit metadata, image digests, or artifact hashes.
- Browser, API, smoke, or command-line checks of the affected path.
- Cache behavior when CDN, static hosting, or split traffic is involved.

If the live target does not match the exact revision, investigate wrong branch, stale cache, failed rollout, background deploy, or traffic splitting. Do not report success until live evidence matches.

## Stop Conditions

Stop before production mutation when:

- The production path is ambiguous.
- The latest deployed revision cannot be identified.
- Related drift cannot be classified.
- Required credentials or provider access are missing.
- The plan would overwrite unrelated work.
- Verification would require destructive production actions.
- Secrets would need to be exposed.

## Anchor Report

End with:

- Production path used.
- Previous production revision.
- Branch and exact revision shipped.
- Deployment, release, build, artifact, or image identifier.
- Drift found and classification.
- Related drift included.
- Skipped drift with reasons.
- Checks run before deploy.
- Live production verification evidence.
- Final tree status.
- Residual risks or follow-up checks.
