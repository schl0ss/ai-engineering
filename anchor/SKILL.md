---
name: anchor
description: Anchor parallel coding sessions, branches, worktrees, PRs, and production deploy state before updating production. Use when the user says "anchor" or asks an AI coding agent to reconcile drift across sessions, preserve unrelated work, move only related changes onto the real production deploy path, deploy an exact revision, and verify the live result. Works in local workspaces, cloud coding sandboxes, and CI-like environments by adapting evidence gathering to the access available.
---

# Anchor

## Purpose

Anchor parallel sessions and production deploy state before updating production.

Reconstruct reality from durable sources before acting: local worktrees, git history, remotes, open PRs, CI/deploy records, provider metadata, and live production behavior. The current conversation is useful context, not proof.

## Execution Environments

Use the strongest evidence available in the current environment:

- In a local workspace, inspect local dirty files, available worktrees, local-only branches, remotes, PRs, CI, deploy provider state, and live production.
- In a cloud coding sandbox, assume the checkout may be an isolated clone. Inspect fetched branches, remotes, PRs, CI, deploy provider state, and live production. Do not assume another machine's unpushed local work is visible.
- In a CI-like runner, treat the runner as an evidence source for the checked-out revision, workflow id, artifact, and environment. Use forge and provider APIs to reconstruct broader state.

If an evidence source is unavailable, record it explicitly and substitute the next-best durable source. Hidden local work from another session cannot be reconciled unless it is exposed through a worktree, branch, PR, patch, artifact, issue, or user-provided context.

## Non-Negotiables

- Preserve unrelated dirty work. Do not reset, delete, stash, rewrite, or overwrite work from other sessions unless the user explicitly asks.
- Discover the production deploy path before editing: branch, worktree, CI job, provider, environment, image, artifact, or server path.
- Implement on the branch or worktree production actually deploys from, or deliberately bring the exact related change there.
- Deploy only an exact commit SHA, image digest, build artifact, or release identifier that can be reported later.
- Verify the live target contains the change after deployment. Provider success alone is not enough.
- Stop and ask when production source, deploy target, or related drift cannot be determined with confidence.

## Workflow

### 1. Load Project Rules

Read repository instructions before changing files. Prefer, in order when present:

- `AGENTS.md`
- `CLAUDE.md`
- `.cursor/rules/*`
- `docs/agents/*`
- contributing, deploy, release, or runbook docs

Use local rules to identify canonical commands, deploy policies, branch names, and verification expectations.

### 2. Build The State Map

Inspect durable state before editing:

- Current repo path, branch, upstream, status, dirty files, untracked files, and unpushed commits.
- Available local worktrees with `git worktree list`, when the environment exposes them.
- Recent branches and commits touching the requested area.
- Remote branches and open PRs when GitHub, GitLab, Bitbucket, or another forge is available.
- CI state for relevant branches and commits.
- Production deploy source: branch, workflow, provider, environment, artifact, image digest, release id, or server checkout.
- Recent production deployments and their associated commits or artifacts.
- Live production URL or endpoint, when discoverable.

Do not trust one source. Cross-check git, forge metadata, deployment metadata, and repo configuration.

### 3. Audit Drift

Compare the intended change against every active source of truth:

- Current branch versus production deploy source.
- Production deployed commit or artifact versus the production branch head.
- Other accessible worktrees, branches, and PRs versus the current worktree or cloud checkout.
- Open PR branches versus the files, routes, modules, services, or assets being changed.
- Recent commits touching the same area that are not in the production deploy path.

Classify each discovered difference:

- `related`: should be included before shipping because it affects the same behavior or dependency chain.
- `conflicting`: touches the same area but cannot be safely merged without human judgment.
- `unrelated`: must be left untouched.
- `unknown`: requires more inspection or a stop for user confirmation.

Report material drift before editing if it changes the execution plan.

### 4. Converge Related Work

Bring only clearly related work onto the production deploy path.

Use the repo's normal integration method where possible: merge, rebase, cherry-pick, patch application, or PR branch update. Preserve authorship and commit history when that matters to the project. Avoid broad sync operations that sweep in unrelated changes.

If related work conflicts, stop with a concise explanation of the competing commits, branches, files, and risk.

### 5. Implement The Requested Change

Make the requested production change on the verified deploy path. Keep edits tightly scoped. Continue to protect unrelated dirty work, including files in the same worktree that were not part of this request.

Run the repo's relevant checks. Prefer the smallest useful verification first, then broader checks when the blast radius justifies them.

### 6. Commit And Publish

Commit only the intended changes and related converged commits. Verify the staged diff before committing. Push the branch that production deploys from, or the branch required by the repo's release process.

Record:

- Branch name.
- Commit SHA.
- Tree status after commit.
- Any skipped branches, commits, or dirty files with reasons.

### 7. Deploy The Exact Revision

Deploy the exact revision through the repo's real production path. Use the provider or release mechanism the project already uses. Examples include CI environments, deployment records, hosting provider releases, container image rollouts, static asset publishing, package releases, or server pull-and-restart scripts.

Capture the strongest available deployment identifier:

- Deployment id.
- Release id.
- Build id.
- Image digest.
- Artifact hash.
- Workflow run id.
- Server commit SHA.

### 8. Verify Live Production

Verify the live target independently of deploy success.

Choose checks that prove the change is present:

- Fetch live HTML, JavaScript, CSS, static assets, API responses, headers, health checks, or version endpoints.
- Compare asset hashes, source maps, build ids, commit metadata, or image digests when available.
- Exercise the affected UI or API path with browser automation, curl, provider logs, or smoke tests.
- Check cache behavior when CDN or static hosting is involved.

If verification fails, investigate whether the wrong branch, stale cache, failed rollout, background deploy, or split traffic is responsible. Do not declare success until production evidence matches the shipped revision.

## Final Report

End with a concise anchor report:

- Production path used.
- Branch and commit SHA shipped.
- Deployment, release, build, artifact, or image identifier.
- Drift found and how it was handled.
- Skipped branches, commits, PRs, or dirty files with reasons.
- Verification evidence from live production.
- Final tree status.
- Any residual risks or follow-up checks.

## Hard Stops

Stop before mutating production when:

- Production deploy source is ambiguous.
- Related drift cannot be confidently classified.
- Required credentials or provider access are missing.
- The deploy path would overwrite unrelated work.
- Verification would require destructive production actions.
- Secrets would need to be exposed in logs or chat.
