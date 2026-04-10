Upgrade repo-gardener to the latest release.

## 1. Read installed version

```bash
if [ ! -f .claude/gardener-config.yaml ]; then
  echo "❌ repo-gardener is not installed in this directory."
  echo "Run /gardener-onboarding first."
  exit 1
fi

installed_version=$(grep -E '^installed_version:' .claude/gardener-config.yaml | sed 's/.*: *//' | tr -d '"' || true)
if [ -z "$installed_version" ]; then
  installed_version="(unknown)"
fi
```

## 2. Resolve latest release tag

```bash
latest_version=$(gh api /repos/agent-team-foundation/repo-gardener/releases/latest --jq .tag_name)

if [ -z "$latest_version" ]; then
  echo "❌ Could not resolve latest release. Check network or GitHub status."
  exit 1
fi
```

## 3. Compare versions

```bash
echo "Installed: $installed_version"
echo "Latest:    $latest_version"

if [ "$installed_version" = "$latest_version" ]; then
  echo "✓ Already on the latest version."
  exit 0
fi
```

## 4. Show release notes and confirm

Show the release notes for `$latest_version` so the user knows what's
changing:

```bash
gh release view "$latest_version" --repo agent-team-foundation/repo-gardener
```

Ask the user:
"Upgrade from `$installed_version` to `$latest_version`?
This will fetch new command files, update `.claude/gardener-config.yaml`,
and commit + push to the config repo. (y/n)"

If `n` → exit cleanly with no changes.

## 5. Fetch new command files

```bash
BASE="https://raw.githubusercontent.com/agent-team-foundation/repo-gardener/${latest_version}"
mkdir -p .claude/commands

for cmd in gardener-manual gardener-schedule gardener-start gardener-loop \
           gardener-stop gardener-onboarding gardener-watch gardener-upgrade; do
  curl -fsSL -o ".claude/commands/${cmd}.md" "${BASE}/.claude/commands/${cmd}.md"
done

# Verify none are empty
for f in .claude/commands/gardener-*.md; do
  if [ ! -s "$f" ]; then
    echo "❌ Download failed: $f is empty. Aborting upgrade."
    exit 1
  fi
done
```

## 6. Update installed_version in config

```bash
if grep -q '^installed_version:' .claude/gardener-config.yaml; then
  # Replace existing line (BSD/GNU sed compatible)
  sed -i.bak "s|^installed_version:.*|installed_version: ${latest_version}|" .claude/gardener-config.yaml
  rm -f .claude/gardener-config.yaml.bak
else
  echo "installed_version: ${latest_version}" >> .claude/gardener-config.yaml
fi
```

## 7. Commit and push

```bash
current_branch=$(git rev-parse --abbrev-ref HEAD)
default_branch=$(gh repo view --json defaultBranchRef --jq .defaultBranchRef.name)

git add .claude/commands/gardener-*.md .claude/gardener-config.yaml
git diff --cached --quiet && {
  echo "No file changes to commit (already on $latest_version files)."
  exit 0
}
git commit -m "chore: upgrade repo-gardener to ${latest_version}"
```

If `current_branch == default_branch` → `git push`.

If `current_branch != default_branch` → ask:

"You're on `$current_branch`, but the cloud schedule reads from
`$default_branch`. Push directly to `$default_branch`, open a PR, or
push to current branch only? (1/2/3)"

- 1 → `git push origin HEAD:$default_branch`
- 2 → `git push -u origin $current_branch && gh pr create --base $default_branch --title "chore: upgrade repo-gardener to ${latest_version}" --body "Auto-upgrade via /gardener-upgrade."`
- 3 → `git push -u origin $current_branch`, warn that the cloud schedule
  will run the old version until merged.

## 8. Restart instructions

Output:
"✓ Upgraded from `$installed_version` to `$latest_version`.

**Important**: restart Claude Code (or start a new session) so the
updated slash commands are picked up by the registry. After restarting,
all `/gardener-*` commands will use the new version."
