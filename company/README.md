# GroomBook

> An open source business management solution for pet groomers.

![Org Chart](images/org-chart.png)

## What's Inside

> This is an [Agent Company](https://agentcompanies.io) package from [Paperclip](https://paperclip.ing)

| Content | Count |
|---------|-------|
| Agents | 5 |
| Projects | 5 |
| Skills | 9 |

### Agents

| Agent | Role | Reports To |
|-------|------|------------|
| Flea Flicker | Engineer | the-dogfather |
| Lint Roller | qa | the-dogfather |
| Pawla Abdul | CMO | scrubs-mcbarkley |
| Scrubs McBarkley | CEO | — |
| The Dogfather | CTO | scrubs-mcbarkley |

### Projects

- **GroomBook App** — This git repository is the primary GroomBook Application source code and associated build artifacts.
- **GroomBook Infra** — This repository is the infrastructure associated with the development and production/demo instances of GroomBook.  It is a target gitrepository of a 2 step Flux GitOps process that is triggered from an external kubernetes cluster management repository.
- **GroomBook Org** — This repository houses the organization level GitHub Pages as well as shared GitHub Actions.
- **GroomBook Site** — This repository houses the primary GitHub Pages based site for the GroomBook Platform.
- **Onboarding**

### Skills

| Skill | Description | Source |
|-------|-------------|--------|
| github-app-token | Generate a GitHub installation access token from a GitHub App PEM key, App ID, and Installation ID, then authenticate the gh CLI with it. | [github](https://github.com/cpfarhood/skills) |
| flux-controller-patch-releases | > | [github](https://github.com/fluxcd/agent-skills) |
| gitops-cluster-debug | > | [github](https://github.com/fluxcd/agent-skills) |
| gitops-knowledge | > | [github](https://github.com/fluxcd/agent-skills) |
| gitops-repo-audit | > | [github](https://github.com/fluxcd/agent-skills) |
| paperclip-create-agent | > | [github](https://github.com/paperclipai/paperclip/tree/master/skills/paperclip-create-agent) |
| paperclip-create-plugin | > | [github](https://github.com/paperclipai/paperclip/tree/master/skills/paperclip-create-plugin) |
| paperclip | > | [github](https://github.com/paperclipai/paperclip/tree/master/skills/paperclip) |
| para-memory-files | > | [github](https://github.com/paperclipai/paperclip/tree/master/skills/para-memory-files) |

## Getting Started

```bash
pnpm paperclipai company import this-github-url-or-folder
```

See [Paperclip](https://paperclip.ing) for more information.

---
Exported from [Paperclip](https://paperclip.ing) on 2026-03-26
