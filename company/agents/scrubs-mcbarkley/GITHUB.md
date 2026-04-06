# GitHub

#### GitHub is the primary source of truth.  Paperclip issues must have a corresponding GitHub issue, if one does not exist it should be created. Both GitHub and Paperclip issues should remain open until the work is completed, reviewed, approved, merged, and quality assurance has been performed.

### You have GitHub access via a GitHub App with credentials stored in a file and environment variables. A GitHub MCP server and the gh cli are available.
All changes must happen via pull request.
Tag @cpfarhood in all pull requests for **visibility only** (cc, not review request).

### GitHub Authentication

**Invoke the `github-app-token` skill** before any GitHub operation. The skill generates a short-lived installation token, writes it to `$AGENT_HOME/.gh-token`, and authenticates via `gh auth login --with-token`. Follow whatever the skill says.

**NEVER run `gh auth login` interactively.** The interactive device-auth flow hangs headless agents for minutes. The skill uses `gh auth login --with-token < "$AGENT_HOME/.gh-token"` which is non-interactive and correct. Clean up the token file after use with `rm -f "$AGENT_HOME/.gh-token"`.

> **Token expiry:** The generated token expires after ~1 hour. Re-invoke the skill to regenerate if your session runs long enough that it may have expired.

### Creating Pull Requests

Use the `gh` CLI or the GitHub MCP server to create pull requests. Always cc @cpfarhood for visibility — do **not** request review from @cpfarhood.

```bash
gh pr create --title "..." --body "... cc @cpfarhood"
```

### PR Review & Merge Policy

There are **three merge points** corresponding to three environments. Each has different reviewers and a different authorized merger.

#### Dev merge (Engineer → Dev branch)
- **Reviewer:** QA (Lint Roller) — code quality review and GitHub approval
- **Merger:** QA (Lint Roller)
- **Result:** Auto-deploys to `groombook-dev`

#### UAT merge (Dev → UAT branch)
- **Reviewers:** QA (Lint Roller) + CTO (The Dogfather)
- **Merger:** CTO (The Dogfather)
- **Result:** Auto-deploys to `groombook-uat`; Shedward then validates the live UAT environment

#### Production merge (UAT → Production branch)
- **Prerequisites:** Shedward UAT sign-off + Barkley security review sign-off
- **Merger:** CEO (Scrubs McBarkley) — sole authorized agent for production merges
- **Result:** Auto-deploys to `groombook` (production)

**@cpfarhood is not a reviewer.** Do not request review from or tag @cpfarhood as a required approver. The board is cc'd for visibility only (`cc @cpfarhood` in PR body).

> **Note:** Agents have read/write access to dev and UAT environments. Production merges require CEO authorization only after UAT and security gates are cleared.