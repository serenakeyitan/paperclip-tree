Start gardener sync automation for this tree repo.

gardener sync detects when bound source repos have drifted from the
tree and opens PRs to update it. It also runs repo-gardener to review
those PRs for context fit.

## 0. Version check

Read and execute `.claude/commands/gardener-version-check.md` as a
runbook. If it exits, stop here — do not proceed. If it warns but
continues, proceed.

## 1. Verify installed

Check that `.claude/commands/gardener-sync-manual.md` exists locally.

- If missing → output:
  "❌ gardener sync is not installed. Run `/gardener-onboarding` to
  install the sync module, then retry."
  STOP.

## 2. Verify bindings exist

Check that `.first-tree/bindings/` exists and contains at least one
`.json` file.

- If empty or missing → output:
  "❌ No bound sources found in `.first-tree/bindings/`.
  Bind a source repo first with `first-tree bind`, then retry."
  STOP.

## 3. Verify bindings have remoteUrl

Read each `.json` file in `.first-tree/bindings/`. For each one,
check that `remoteUrl` is a non-empty string.

- If any binding is missing `remoteUrl` → output:
  "❌ Binding `<sourceId>` has no `remoteUrl`.
  Edit `.first-tree/bindings/<sourceId>.json` and add:
    \"remoteUrl\": \"https://github.com/<owner>/<repo>\"
  Then retry."
  STOP (do not partially start — all bindings must be valid).

## 4. Verify gardener is installed

Check that `.claude/commands/gardener-comment-manual.md` exists locally.

- If missing → output:
  "❌ repo-gardener is required for sync to review tree-update PRs.
  Install it first:

  Fetch the latest release of repo-gardener and execute its onboarding
  script: https://github.com/agent-team-foundation/repo-gardener/releases/latest

  After installing, re-run `/gardener-sync-start`."
  STOP.

## 5. Verify config is on the default branch

The cloud schedule reads files from the default branch. Check that
the sync runbook and bindings are committed and pushed:

```bash
current_branch=$(git rev-parse --abbrev-ref HEAD)
default_branch=$(gh api "/repos/$(gh repo view --json nameWithOwner --jq .nameWithOwner)" --jq .default_branch)
```

Check if `.claude/commands/gardener-sync-manual.md` exists on the default
branch:

```bash
repo_name=$(gh repo view --json nameWithOwner --jq .nameWithOwner)
gh api "/repos/$repo_name/contents/.claude/commands/gardener-sync-manual.md?ref=$default_branch" \
  --silent 2>/dev/null
on_default=$?
```

- If `on_default == 0` → good, proceed to Step 6.
- If `on_default != 0` → ask the user:

  "⚠️ Sync runbook is not on the default branch (`$default_branch`).
  The cloud schedule needs it there.

  Options:
  1. **Merge/push first**, then retry `/gardener-sync-start`.
  2. **Start loop only** — skip cloud schedule. Local loop works
     from any branch. Start the schedule later once merged.

  Which?"

  - Option 1 → STOP.
  - Option 2 → set `SKIP_SCHEDULE=true`, proceed.

## 6. Verify GitHub MCP connector

The cloud schedule has no `gh` CLI — it uses the GitHub MCP connector.
The **local loop** uses `gh` CLI and works without any connector, so
connector failures here are non-fatal: set `SKIP_SCHEDULE=true`,
warn, and continue to Step 8 (local loop).

### 6a. Is the connector present?

Attempt `mcp__github__get_me` (or any `github*` tool ending in
`get_me`).

- Not found / unknown tool / 401 → set `SKIP_SCHEDULE=true`, print:

  > ⚠️ **GitHub MCP connector not connected — cloud schedule will be skipped.**
  >
  > The local loop will still run (uses `gh` CLI). To enable the
  > hourly cloud schedule later, connect a GitHub MCP connector at
  > https://claude.ai/settings/connectors with **Contents: read**,
  > **Issues: write**, and **Pull requests: write** scopes on this
  > tree repo and all bound source repos, then re-run
  > `/gardener-sync-start`.

  Skip Step 6b, continue to Step 7.

### 6b. Do the repos pass?

Build the probe list: this tree repo + every bound source repo
(from the `remoteUrl` fields in the binding files).

Call `mcp__github__get_repository` against each. Collect failures.

If any fail → set `SKIP_SCHEDULE=true`, print:

  > ⚠️ **GitHub MCP connector is missing the following repo(s) — cloud schedule will be skipped:**
  > - `<repo>` (error: <short error>)
  >
  > The local loop will still run. Add these at
  > https://claude.ai/settings/connectors, then re-run
  > `/gardener-sync-start`.

Continue to Step 7.

## 7. Start cloud schedule (unless SKIP_SCHEDULE=true)

Use `RemoteTrigger` tool with `action: "list"`. Look for a trigger
named `gardener-sync`.

- Found and enabled → skip. Log: "Schedule already running."
- Found and disabled → `RemoteTrigger` `action: "update"`,
  `body: {"enabled": true}`.
- Not found → `RemoteTrigger` `action: "create"` with:
  - `name`: "gardener-sync"
  - `cron_expression`: "0 * * * *" (every hour)
  - Prompt: contents of `.claude/commands/gardener-sync-schedule.md`,
    prefixed with `CONFIG_REPO=<this-tree-repo>` so the cloud agent
    knows which repo to read bindings from.
  - Source repo: this tree repo's GitHub URL

## 8. Start local loop

Start: `/loop 10m /gardener-sync-loop`

## 9. Confirm

Output:
"🌳 gardener sync is running.
- Tree repo: `<this repo>`
- Bound sources: <list of sourceId → remoteUrl>
- Cloud schedule: <every hour | SKIPPED — config not on default branch | SKIPPED — GitHub MCP connector not connected>
- Local loop: every 10min (runs while you're here)
- Stop anytime: `/gardener-sync-stop`"
