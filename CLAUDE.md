# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

This is the **GitHub org-level configuration repository** (`groombook/.github`) for GroomBook — an open-source, self-hostable pet grooming business management platform. It contains:

- `profile/` — GitHub organization profile README and logo
- `company/` — Paperclip AI company configuration export (agent definitions, skills, projects)

There is no application code, build system, or test suite here. This repo is purely configuration and documentation.

## Related Repositories

| Repo | Purpose |
|------|---------|
| `groombook/groombook` | Primary application (TypeScript, Node.js, React, PostgreSQL) |
| `groombook/agents` | Canonical agent definitions — prompts, personas, heartbeats, adapter configs |
| `groombook/infra` | Kubernetes manifests for Flux GitOps deployment |

## Company Directory (`company/`)

This is an export from [Paperclip](https://paperclip.ing) and contains a snapshot of the agent company configuration:

- `.paperclip.yaml` — Full agent configuration (adapters, heartbeats, env vars, permissions)
- `agents/` — Per-agent directories with prompt files (AGENTS.md, SOUL.md, HEARTBEAT.md, etc.)
- `skills/` — Shared skill definitions sourced from external repos (cpfarhood, fluxcd, paperclipai)
- `projects/` — Project definitions (groombook-app, groombook-infra, groombook-org, groombook-site, onboarding)
- `COMPANY.md` — Company metadata frontmatter

The canonical source for agent configurations is the `groombook/agents` repo. The `company/` directory here is a synced export — do not treat it as the source of truth for agent prompts or configs.

## Key Policies

- **Container images**: `ghcr.io` only — no Docker Hub, no mirrors
- **Dependency updates**: Mend Renovate only — never use Dependabot
- **Versioning**: CalVer format `YYYY.MDD.PATCH` (e.g., `2026.318.0`), not SemVer
- **All PRs**: Include `cc @cpfarhood` at the bottom of the PR body
