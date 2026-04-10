Set up repo-gardener for this project. Do every step in order.

repo-gardener is a **context-aware review bot** — it comments on PRs
and issues with product/context fit analysis, reading from your
project's context tree. It does NOT push code or open PRs.

## 0. Pick install mode

Before anything else, ask the user:

"What's your goal with repo-gardener?

1. **Review my own repo** (Maintainer mode)
   You maintain a repo and want gardener to review its own PRs and
   issues against a context tree.
   → Install goes into your own repo.
   → You should run onboarding from **inside your project directory**.

2. **Review someone else's OSS repo** (External reviewer mode)
   You want gardener to comment on an upstream OSS project you don't
   maintain. Config lives in a repo you DO own (usually your context
   tree); gardener reviews the target via GitHub API.
   → Install goes into your config repo (the tree repo).
   → You should run onboarding from **inside your tree repo**, not
   inside the target repo.

Which?"

Wait for the answer. Remember the choice as `INSTALL_MODE=maintainer`
or `INSTALL_MODE=external`.

## 1. Verify you're in the right directory

Run `git rev-parse --show-toplevel` and `gh repo view --json nameWithOwner`
to detect the current repo.

**If not inside a git repo**:
- Maintainer mode → ask: "Please `cd` into the project you want to
  review, then re-run `/gardener-onboarding`." STOP.
- External mode → ask: "Please `cd` into the repo that will host the
  gardener config (usually your context tree), then re-run
  `/gardener-onboarding`." STOP.

**If inside a git repo**, confirm with the user:
- Maintainer mode: "Detected current repo: `<owner/name>`. Is this the
  repo you want gardener to review? (y/n)"
  - `n` → "Please `cd` into the correct repo, then re-run." STOP.
- External mode: "Detected current repo: `<owner/name>`. Is this the
  repo where gardener config should live (typically your context tree
  repo, not the target repo)? (y/n)"
  - `n` → "Please `cd` into your config/tree repo, then re-run." STOP.

## 2. Install gardener commands

Resolve the **latest release tag** at install time so users always
get the newest version without editing this file.

```bash
GARDENER_VERSION=$(gh api /repos/agent-team-foundation/repo-gardener/releases/latest --jq .tag_name)
if [ -z "$GARDENER_VERSION" ]; then
  echo "❌ Could not resolve latest release. Check network or GitHub status."
  exit 1
fi
echo "Installing repo-gardener $GARDENER_VERSION"

BASE="https://raw.githubusercontent.com/agent-team-foundation/repo-gardener/${GARDENER_VERSION}"
mkdir -p .claude/commands
for cmd in gardener-manual gardener-schedule gardener-start gardener-loop \
           gardener-stop gardener-onboarding gardener-watch gardener-upgrade; do
  curl -fsSL -o ".claude/commands/${cmd}.md" "${BASE}/.claude/commands/${cmd}.md"
done
```

Verify downloads are not empty:

```bash
for f in .claude/commands/gardener-*.md; do
  if [ ! -s "$f" ]; then
    echo "❌ Download failed: $f is empty. Check your network and try again."
    exit 1
  fi
done
```

## 3. Set target_repo (and config_repo if external)

The install mode was already chosen in Step 0.

**Maintainer mode** (`INSTALL_MODE=maintainer`):
- `target_repo=<current owner/name from gh repo view>`
- `config_repo` is unset (defaults to `target_repo`)
- Both live in the current repo (where you ran onboarding)

**External reviewer mode** (`INSTALL_MODE=external`):
- Current repo = `config_repo` (already verified in Step 1)
- `config_repo=<current owner/name from gh repo view>`
- Ask: "Which repo should gardener review? Paste `owner/name`."
  Validate with `gh repo view <owner>/<name> --json nameWithOwner`.
- `target_repo=<answer>`
- If `target_repo == config_repo`, something's wrong → ask again.

(Config file is written at the end of Step 4, after the tree URL is resolved.)

## 4. Find the context tree

Try these sources in order, picking the first match:

1. **Target repo's CLAUDE.md / AGENTS.md** (if `target_repo` is the
   current repo, grep local files; if it's a different repo, use
   `gh api /repos/$target_repo/contents/CLAUDE.md` and
   `gh api /repos/$target_repo/contents/AGENTS.md` — decode base64).
2. **User's global `~/CLAUDE.md`** (`cat ~/CLAUDE.md 2>/dev/null`).
3. **Ask the user**.

For sources 1 and 2, use this regex to extract GitHub URLs:

```bash
grep -oE '(https://)?github\.com/[a-zA-Z0-9._-]+/[a-zA-Z0-9._-]+'
```

Prefer URLs where the surrounding ~200 characters contain one of:
`tree`, `context`, `memory`, `session`, `first-tree`, `kael-tree`.

**Always confirm the URL with the user** before saving — even when
auto-detected — to avoid false positives. Show the detected URL and ask
"Use this as your context tree? (y/n/different)".

**If no URL is found or the user says "different"**, ask:

"🌳 No context tree found. Gardener requires a context tree to
function — it reviews PRs/issues against a tree repo that holds your
project's design decisions, conventions, and constraints in markdown.
Without a tree there is nothing to review against.

Choose one:

1. **I already have a tree repo** — paste the URL (`github.com/<org>/<repo>`)
2. **Build a tree first** — First-Tree is a companion CLI that sets
   up your context tree interactively:
   https://github.com/agent-team-foundation/first-tree
   Run `first-tree init` to create the tree, push it to GitHub, then
   re-run `/gardener-onboarding` and come back to this step with the
   tree URL."

- Option 1 → validate with `gh repo view <owner>/<name>`,
  set `tree_repo=<url>`, continue onboarding
- Option 2 → STOP. Output:
  "🌳 Onboarding paused. Run `first-tree init` to build your tree,
   push it to GitHub, then re-run `/gardener-onboarding` from this
   same directory and paste the tree URL when prompted."

Write the config. In **maintainer mode**, omit `config_repo` (it
defaults to `target_repo`). In **external reviewer mode**, set
`config_repo` explicitly. Always include `installed_version` so
`/gardener-upgrade` can detect when an update is available.

```bash
# Maintainer mode:
cat > .claude/gardener-config.yaml <<EOF
target_repo: <owner>/<name>
tree_repo: <tree-url>
installed_version: ${GARDENER_VERSION}
EOF

# External reviewer mode:
cat > .claude/gardener-config.yaml <<EOF
target_repo: <target-owner>/<target-name>
tree_repo: <tree-url>
config_repo: <config-owner>/<config-name>
installed_version: ${GARDENER_VERSION}
EOF
```

## 5. Add the tree cache directory to .gitignore

```bash
grep -q '.gardener-tree-cache' .gitignore 2>/dev/null || \
  echo '.gardener-tree-cache/' >> .gitignore
```

## 6. Commit and push

The cloud schedule clones the **default branch** of `config_repo`. In
maintainer mode `config_repo == target_repo`; in external reviewer
mode `config_repo` is a repo the user owns (usually the tree repo).

Either way, the push is to `config_repo`'s default branch — and since
in external reviewer mode the user owns that repo, pushing directly is
expected. In maintainer mode the branch protection story still applies
(see the three options below).

```bash
current_branch=$(git rev-parse --abbrev-ref HEAD)
default_branch=$(gh repo view --json defaultBranchRef --jq .defaultBranchRef.name)

git add .claude/commands/gardener-*.md .claude/gardener-config.yaml .gitignore
git diff --cached --quiet && echo "No changes to commit" || \
  git commit -m "chore: install repo-gardener ${GARDENER_VERSION}"
```

If `current_branch` == `default_branch` → `git push`.

If `current_branch` != `default_branch` → ask the user:

"You're on `$current_branch`, but the cloud schedule reads from
`$default_branch`. How do you want to install?

1. **Push this branch and open a PR** — recommended for team repos
   with branch protection
2. **Push directly to `$default_branch`** — only works if you have
   permission
3. **Push to current branch only** — cloud schedule won't work until
   you merge later; local loop still works

Which?"

- Option 1 → `git push -u origin $current_branch`, then
  `gh pr create --base $default_branch --title "chore: install repo-gardener" --body "Installs repo-gardener."`
- Option 2 → `git push origin HEAD:$default_branch`
- Option 3 → `git push -u origin $current_branch`, warn that
  `/gardener-start` will skip the schedule until merged

## 7. Test run (optional)

Ask the user:
"⚠️ The test run will scan open PRs and issues on `<target_repo>` and
may post real comments. Proceed?"

- Yes → execute `.claude/commands/gardener-manual.md` once.
- No → skip to Step 8.

## 8. Confirm

Output:
"🌱 repo-gardener ${GARDENER_VERSION} installed.
- Target repo: `<target_repo>`
- Commands and config committed and pushed to remote.

**Important**: restart Claude Code (or start a new session) so the new
slash commands are picked up. After restarting:

- `/gardener-start` — start automation (loop + schedule)
- `/gardener-manual` — one-off review
- `/gardener-watch` — open a terminal popup that tails run logs live (clickable URLs)
- `/gardener-upgrade` — auto-update to the latest release
- `/gardener-stop` — pause everything"
