Start repo-gardener automation for this project.

repo-gardener v2.0 is a context-aware review bot. It comments on PRs
and issues with tree-backed product/context fit analysis. It never
pushes code.

## 1. Verify installed

Check that `.claude/commands/gardener-manual.md` and
`.claude/gardener-config.yaml` both exist locally.

- If either missing → output:
  "❌ repo-gardener is not installed. Run `/gardener-onboarding` first."
  STOP.

## 2. Resolve config_repo

Read `.claude/gardener-config.yaml`. Determine the config repo:

```bash
target_repo=$(yq .target_repo .claude/gardener-config.yaml)
config_repo=$(yq .config_repo .claude/gardener-config.yaml)
if [ -z "$config_repo" ] || [ "$config_repo" = "null" ]; then
  config_repo="$target_repo"
fi
```

`config_repo` is where the config file lives and the cloud schedule
will clone it from. In maintainer mode it equals `target_repo`; in
external reviewer mode it's a separate repo the user owns.

## 3. Verify config is on config_repo's default branch

```bash
default_branch=$(gh api "/repos/$config_repo" --jq .default_branch)
gh api "/repos/$config_repo/contents/.claude/gardener-config.yaml?ref=$default_branch" \
  --silent 2>/dev/null
config_on_default=$?
```

- If `config_on_default == 0` → good, skip to Step 4.
- If `config_on_default != 0` → config is not on the default branch
  yet. Ask the user:

  "⚠️ `.claude/gardener-config.yaml` is not yet on `$config_repo`'s
  default branch (`$default_branch`). The cloud schedule can only
  read files from the default branch.

  Options:
  1. **Merge/push first**, then retry `/gardener-start`.
  2. **Start loop only** — skip the cloud schedule for now. Local loop
     works from any branch. Start the schedule later once merged.
  3. **Open a PR for me** — create a PR from the current branch into
     `$default_branch` with the gardener install commit.

  Which?"

  - Option 1 → STOP.
  - Option 2 → set `SKIP_SCHEDULE=true`, proceed.
  - Option 3 →
    `gh pr create --repo $config_repo --base $default_branch --head $(git rev-parse --abbrev-ref HEAD) --title "chore: install repo-gardener" --body "Installs repo-gardener commands and config."`
    then STOP.

## 4. Start cloud schedule (unless SKIP_SCHEDULE=true)

Use `RemoteTrigger` tool with `action: "list"`. Look for a trigger
named `repo-gardener`.

- Found and enabled → skip. Log: "Schedule already running."
- Found and disabled → `RemoteTrigger` `action: "update"`,
  `body: {"enabled": true}`.
- Not found → `RemoteTrigger` `action: "create"` with:
  - `name`: "repo-gardener"
  - `cron_expression`: "0 * * * *" (every hour)
  - Prompt: contents of `.claude/commands/gardener-schedule.md`,
    prefixed with `CONFIG_REPO=<config_repo>` so the cloud agent
    knows which repo to clone for config.
  - Source repo: `https://github.com/$config_repo` (NOT `target_repo`)

## 5. Start local loop

Start: `/loop 10m /gardener-loop`

## 6. Confirm

Output:
"🌱 repo-gardener is running.
- Target repo (reviewed): `$target_repo`
- Config repo (where gardener lives): `$config_repo`
- Cloud schedule: <every hour | SKIPPED — config not on default branch>
- Local loop: every 10min (runs while you're here)
- Stop anytime: `/gardener-stop`"
