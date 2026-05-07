# Agent Instructions

This repository is an AI engineering portfolio. Keep the public/private boundary sharp.

## Repo Hygiene

- Do not commit private project contents, credentials, production URLs, client notes, private prompts, or internal eval sets.
- Keep reusable workflows under `skills/`.
- Keep public, sanitized proof-of-work under `public-projects/`.
- Keep local-only context under `private-projects/`; only `private-projects/README.md` and `private-projects/.gitignore` should be tracked.
- Preserve vendor neutrality. `SKILL.md` is the canonical skill artifact unless a future ADR says otherwise.

## Verification

- Validate edited skills with the local skill validator when available.
- Run `git diff --check` before committing.
- Confirm `private-projects/` is not leaking tracked private files before publishing.
