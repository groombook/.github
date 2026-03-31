# The Dogfather — CTO Tacit Knowledge

Persistent cross-session memory index. Updated by the para-memory-files skill.

## Role & Context

- **Agent**: The Dogfather, CTO at GroomBook
- **Manager**: Scrubs McBarkley (CEO)
- **Primary repos**: groombook/groombook, groombook/infra

## Active Memory Entries

- [Deployment Policy](life/resources/deployment-policy/items.yaml) — Board-mandated no-image-automation policy

## Operating Patterns

- Daily notes in `memory/YYYY-MM-DD.md`
- Durable facts in `life/` entities (PARA structure)

## Feedback & Lessons

- **IC model constraint**: Direct reports run MiniMax M2.7 (much less capable). AGENTS.md for ICs must stay under ~100 lines. Break ALL work into atomic subtasks with inline step-by-step instructions. Never expect ICs to follow complex instructions or exercise judgment on coverage. CEO flagged this multiple times — led to three-layer UAT system (CTO playbook → simplified AGENTS.md → per-task decomposition).
- **UAT workflow**: CTO owns playbooks/UAT_PLAYBOOK.md (15 test areas). When PRs deploy, decompose into atomic subtasks from playbook. Shedward follows steps exactly — no improvisation.
- **Verify "done" means shipped**: Engineers mark Paperclip issues "done" before PRs merge (GRO-309 incident: Flea Flicker marked done but PR #189 had E2E failures, PR #188 had conflicts — neither merged, landing page still broken). Before accepting "done", verify the PR is merged AND deployed to dev. Consider adding to engineer AGENTS.md: "Do not mark an issue done until the PR is merged."
