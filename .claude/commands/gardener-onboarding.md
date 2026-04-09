Set up repo-gardener for this project. Do every step in order.

repo-gardener is a **context-aware review bot** — it comments on PRs
and issues with product/context fit analysis, reading from your
project's context tree. It does NOT push code or open PRs.

## 0. Verify repo

Check if you are inside a git repository (`git rev-parse --show-toplevel`).

- If NOT inside a repo → ask the user: "Which local directory should I
  use as the working repo? Please cd there first, then re-run
  `/gardener-onboarding`."
  STOP.

## 1. Install gardener commands

Fetch from a pinned release tag for integrity.

```bash
GARDENER_VERSION="v2.1.0"
BASE="https://raw.githubusercontent.com/agent-team-foundation/repo-gardener/${GARDENER_VERSION}"
mkdir -p .claude/commands
curl -fsSL -o .claude/commands/gardener-manual.md "${BASE}/.claude/commands/gardener-manual.md"
curl -fsSL -o .claude/commands/gardener-schedule.md "${BASE}/.claude/commands/gardener-schedule.md"
curl -fsSL -o .claude/commands/gardener-start.md "${BASE}/.claude/commands/gardener-start.md"
curl -fsSL -o .claude/commands/gardener-loop.md "${BASE}/.claude/commands/gardener-loop.md"
curl -fsSL -o .claude/commands/gardener-stop.md "${BASE}/.claude/commands/gardener-stop.md"
curl -fsSL -o .claude/commands/gardener-onboarding.md "${BASE}/.claude/commands/gardener-onboarding.md"
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

## 2. Choose install mode and target repo

Ask the user:

"Which install mode?

1. **Maintainer mode** — I maintain this repo (`<current owner/name>`)
   and want gardener to review its own PRs and issues. Install commits
   will land in this repo.
2. **External reviewer mode** — I want gardener to review a repo I
   **don't** maintain (e.g. an OSS project). I'll store the config in
   my own repo (usually my context tree) and gardener will comment on
   the target repo via GitHub API. No commits on the target repo.

Which?"

### Maintainer mode

- `target_repo=<current owner/name from gh repo view>`
- `config_repo` is unset (defaults to `target_repo`)
- Config + commands will be committed to the current repo

### External reviewer mode

- Ask for the **target repo**: "Which repo should gardener review?
  Paste `owner/name`." Validate with `gh repo view <owner>/<name>`.
- `target_repo=<answer>`
- Ask for the **config repo**: "Where should I store the gardener
  config and commands? This must be a repo you own (so you can push
  to its default branch). Usually this is your context tree repo.
  Paste `owner/name`." Validate with `gh repo view`.
- `config_repo=<answer>`
- The install will commit to `config_repo`, NOT `target_repo`.
- **You must have the config repo cloned locally** for this step to
  work. If not cloned yet, tell the user:
  "Please clone `<config_repo>` locally first, then re-run
  `/gardener-onboarding` from inside that clone."
  STOP.
- After the user has cloned it, they should be inside the `config_repo`
  checkout. Verify with `gh repo view --json nameWithOwner` that the
  current directory matches `config_repo`.

(Config file is written at the end of Step 3, after the tree URL is resolved.)

## 3. Find the context tree

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

"🌳 No context tree found automatically. Gardener needs a tree URL to
do context-aware reviews. Choose one:

1. **Enter a URL** — paste a `github.com/<org>/<repo>` URL
2. **Set up a new First-Tree** — https://github.com/agent-team-foundation/first-tree
   (run `first-tree init`, then re-run `/gardener-onboarding`)
3. **Skip** — install gardener without a tree; reviews will be limited
   until you add one via `.claude/gardener-config.yaml`"

- Option 1 → validate the URL with `gh repo view <owner>/<name>`,
  set `tree_repo=<url>`
- Option 2 → STOP
- Option 3 → set `tree_repo=""` (gardener will mark all verdicts as
  `INSUFFICIENT_CONTEXT` until the tree is set)

Write the config. In **maintainer mode**, omit `config_repo` (it
defaults to `target_repo`). In **external reviewer mode**, set
`config_repo` explicitly:

```bash
# Maintainer mode:
cat > .claude/gardener-config.yaml <<EOF
target_repo: <owner>/<name>
tree_repo: <tree-url-or-empty>
EOF

# External reviewer mode:
cat > .claude/gardener-config.yaml <<EOF
target_repo: <target-owner>/<target-name>
tree_repo: <tree-url>
config_repo: <config-owner>/<config-name>
EOF
```

## 4. Add the tree cache directory to .gitignore

```bash
grep -q '.gardener-tree-cache' .gitignore 2>/dev/null || \
  echo '.gardener-tree-cache/' >> .gitignore
```

## 5. Commit and push

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
  git commit -m "chore: install repo-gardener v2.0.2"
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

## 6. Test run (optional)

Ask the user:
"⚠️ The test run will scan open PRs and issues on `<target_repo>` and
may post real comments. Proceed?"

- Yes → execute `.claude/commands/gardener-manual.md` once.
- No → skip to Step 7.

## 7. Confirm

Output:
"🌱 repo-gardener v2.0.0 installed.
- Target repo: `<target_repo>`
- Commands and config committed and pushed to remote.

**Important**: restart Claude Code (or start a new session) so the new
slash commands are picked up. After restarting:

- Run `/gardener-start` to start automation (loop + schedule).
- Run `/gardener-manual` for a one-off review.
- Run `/gardener-stop` to pause everything."
