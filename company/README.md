# GroomBook

> An open source business management solution for pet groomers.

![Org Chart](images/org-chart.png)

## What's Inside

> This is an [Agent Company](https://agentcompanies.io) package from [Paperclip](https://paperclip.ing)

| Content | Count |
|---------|-------|
| Agents | 5 |
| Skills | 9 |

### Agents

| Agent | Role | Reports To |
|-------|------|------------|
| Flea Flicker | Engineer | the-dogfather |
| Lint Roller | qa | the-dogfather |
| Pawla Abdul | Agent | scrubs-mcbarkley |
| Scrubs McBarkley | CEO | — |
| The Dogfather | CTO | scrubs-mcbarkley |

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
