# SDLC & Source Control

## GitHub Authentication

**Invoke the `github-app-token` skill** before any GitHub operation. It generates a short-lived installation token and sets `GH_TOKEN`.

**Never run `gh auth login`.** It hangs headless agents.

Token expires after ~1 hour. Re-invoke the skill to regenerate if needed.

## Branch Strategy

Three long-lived branches map to the three deployment environments:

| Branch | Environment | Who merges |
|--------|-------------|-----------|
| `dev` | Development | CTO (after QA + CTO approval) |
| `uat` | UAT / Staging | CTO (promotes dev → uat via PR) |
| `main` | Production | CEO (promotes uat → main via PR) |

**Engineers always target `dev`** — never `uat` or `main` directly.

## Pull Requests

All changes must happen via pull request. Always cc @cpfarhood for visibility — not as a reviewer.

```bash
gh pr create --title "..." --body "... cc @cpfarhood"
```

## PR Review & Merge Policy

### Dev branch (`dev`)
Requires **2 approving GitHub reviews** before merge:
1. **QA** (Lint Roller) — quality review and approval
2. **CTO** (The Dogfather) — technical review and approval

CTO review requires QA approval as a precondition.

### UAT branch (`uat`)
Requires **1 approving GitHub review** before merge:
- **CTO** (The Dogfather) — promotes `dev` → `uat` via PR

### Main branch (`main`)
Requires **1 approving GitHub review** before merge:
- **CEO** (Scrubs McBarkley) — promotes `uat` → `main` via PR

@cpfarhood is cc'd for visibility only — never a reviewer.

## Pipeline

```
Dev stage:   Engineer → QA Review → CTO Review → CTO merges PR to dev → [auto deploy Dev]
UAT stage:   CTO opens dev→uat PR → Shedward (regression) → CTO → Barkley (security) → CEO assigned
Prod stage:  CEO merges uat→main PR → [auto deploy Production]
```

### Dev Stage

1. Engineer creates PR targeting `dev`, hands off to QA (Lint Roller): `status: "todo"`
2. QA reviews code and CI. Pass → hand to CTO. Fail → hand back to engineer via CTO.
3. CTO reviews PR. Approve → merge PR into `dev` (triggers auto-deploy to dev). Deny → hand back to engineer.

### UAT Stage

4. CTO opens a PR from `dev` → `uat` to promote the change, assigns Shedward Scissorhands for regression: `status: "todo"`
5. Shedward runs UAT. Pass → reports to CTO. Fail → reports to CTO (CTO cascades to engineer).
6. CTO assigns Barkley Trimsworth for security review: `status: "todo"`
7. Barkley reviews. Pass → CTO assigns to CEO. Fail → CTO cascades to engineer.

### Prod Stage

8. CEO reviews and merges the `uat` → `main` PR → auto-deploy to Production.
9. CEO rejects → returns to CTO → engineer.

### Hierarchy Rules

- CTO rejections go directly to engineer (not through QA).
- Shedward UAT failures go to CTO (not directly to engineer).
- Barkley security failures go to CTO (not directly to engineer).
- CEO rejections go to CTO (not directly to engineer).

## Handoff Protocol — Mandatory

Every handoff to another agent requires ALL THREE steps:

### Step 1 — Explicit Assignment

PATCH the issue with `assigneeAgentId: "<target-agent-uuid>"`.
@mentioning is NOT a handoff — the agent won't wake without explicit assignment.

### Step 2 — Status = `todo`

Every handoff sets `status: "todo"`. Never `in_review` — it doesn't appear in inbox-lite and the target agent won't wake.

### Step 3 — Release Checkout

```
POST /api/issues/{issueId}/release
Headers: Authorization: Bearer $PAPERCLIP_API_KEY, X-Paperclip-Run-Id: $PAPERCLIP_RUN_ID
```

Without this release, the receiving agent cannot checkout the issue.

## Status Semantics

| Status | Meaning |
|--------|---------|
| `backlog` | Not ready; parked or unscheduled |
| `todo` | Ready and actionable; not checked out |
| `in_progress` | Actively owned; enter by checkout only |
| `in_review` | Self-held only; awaiting external feedback |
| `blocked` | Cannot proceed; state blocker and who must act |
| `done` | Complete, no follow-up remains |
| `cancelled` | Intentionally abandoned |

## Status Transition Rules

| Handoff | Correct | Wrong |
|---------|---------|-------|
| Engineer → QA | `todo` | ~~`in_review`~~ |
| QA → CTO | `todo` | ~~`in_review`~~ |
| CTO → CEO | `todo` | ~~`in_review`~~ |
| CTO → Shedward (UAT) | `todo` | ~~`in_review`~~ |
| CTO → Barkley (security) | `todo` | ~~`in_review`~~ |
| Shedward → CTO (fail) | `todo` | ~~`in_review`~~ |
| Barkley → CTO (fail) | `todo` | ~~`in_review`~~ |
