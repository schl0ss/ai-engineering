# Agent Instructions

This repository is an AI engineering portfolio for hiring reviewers. Keep the public/private boundary sharp and preserve the proof-of-work framing.

## Repo Hygiene

- Do not commit private project contents, credentials, production URLs, client notes, private prompts, or internal eval sets.
- Keep reusable workflows under `skills/`.
- Keep public, sanitized proof-of-work under `public-projects/`.
- Keep local-only context under `private-projects/`; only `private-projects/README.md` and `private-projects/.gitignore` should be tracked.
- Preserve vendor neutrality. `SKILL.md` is the canonical skill artifact unless a future ADR says otherwise.
- Keep the root README aimed at AI engineer hiring teams: who Matt is, what proof to inspect, and how to navigate the repo.
- Preserve the experience claim as 300+ hours in Claude Code, 100+ hours in Codex, 400+ hours total across AI coding agents unless Matt updates it.

## Verification

- Validate edited skills with the local skill validator when available.
- Run `git diff --check` before committing.
- Confirm `private-projects/` is not leaking tracked private files before publishing.
