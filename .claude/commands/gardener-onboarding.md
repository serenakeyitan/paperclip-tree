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
for cmd in gardener-comment-manual gardener-comment-schedule gardener-comment-start \
           gardener-comment-loop gardener-comment-stop gardener-comment-watch \
           gardener-sync-manual gardener-sync-schedule gardener-sync-start \
           gardener-sync-loop gardener-sync-stop \
           gardener-start gardener-stop \
           gardener-onboarding gardener-upgrade; do
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

## 3.5. Verify GitHub MCP connector (cloud pre-flight)

**Why this step exists**: `/gardener-start`, `/gardener-comment-schedule`, and
`/gardener-comment-manual` run in Anthropic's cloud when invoked via the remote
schedule. Cloud containers do **not** have `gh` CLI installed — all
GitHub access goes through the GitHub MCP connector the user connects
at https://claude.ai/settings/connectors. If the connector is missing
or scoped to a different repo, every cloud run silently aborts at
Step 4 with `MCP tools restricted to <other-repo>, cannot post to
<target_repo>`. The local `/gardener-comment-loop` keeps working (it uses
`gh`), which masks the problem and makes it look like the schedule is
paused.

Onboarding must refuse to finish unless the connector is wired up
correctly, so the user finds out now instead of hours later when they
notice no comments are posting.

**Do NOT fall back to `gh` CLI if this check fails, even though `gh`
works locally.** The whole point is that the cloud schedule does not
have `gh` — a passing local check means nothing for cloud runs.

### 3.5a. Is a GitHub MCP connector present?

**Concrete probe.** Do not try to introspect your tool list — just
attempt a known GitHub MCP tool call and classify the result:

1. Try `mcp__github__get_me` (no arguments needed). Also accepted:
   `mcp__github_*__get_me`, `mcp__claude_ai_github__get_me` — any
   tool whose name contains `github` and ends in `get_me`.
2. Classify the result:
   - **Tool is not defined at all** (the tool call fails with
     "unknown tool" / "tool not found" / the tool is simply absent
     from your schema) → no connector present. Go to the "no
     connector" STOP below.
   - **Tool exists and returns a user object** → connector present,
     continue to 3.5b.
   - **Tool exists but errors with `401` / `not authenticated`** →
     connector is half-installed. Same treatment as "no connector".
   - **Tool exists but errors with something else** (network, 500)
     → retry once, then if still failing treat as "no connector"
     and tell the user the exact error.

If no connector is present, STOP with this message and **do not
write `.claude/gardener-config.yaml`, do not proceed to Step 4, and
do not continue onboarding**:

> ❌ **GitHub MCP connector not connected.**
>
> repo-gardener runs most commands in Anthropic's cloud, which has no
> `gh` CLI — it needs the GitHub MCP connector.
>
> **Fix**: connect one at https://claude.ai/settings/connectors, then
> re-run `/gardener-onboarding`. Make sure the connector includes
> write access (issues + PR comments) on:
> - `<target_repo>` (the repo gardener will review)
> - `<config_repo>` (only in external reviewer mode)

### 3.5b. Does the connector cover the repos gardener needs?

Build a list of repos to probe based on install mode:

- Maintainer mode: `[target_repo]`
- External reviewer mode: `[config_repo, target_repo]` — in this
  exact order. `config_repo` comes first because the cloud schedule
  **clones `config_repo` at the very start of every run**, so a
  missing `config_repo` is the earlier runtime failure point.

**Probe every repo in the list — do not fail-fast.** Collecting all
failures up front saves the user a re-run cycle when both repos
need to be added.

For each repo, call `mcp__github__get_repository` (or the
connector's equivalent — any `github*` tool ending in
`get_repository`) with `owner` and `repo` parsed from the slug.
Classify:

- Returns a repo object → ✓ visible, remember this repo as OK.
- Returns `403` / `404` / `Resource not accessible` / any
  `MCP tools restricted to ...` error → ✗ not covered, remember
  this repo as FAIL along with the error string.
- Any other error → retry once, then on persistent failure mark as
  FAIL with the error string.

After probing every repo in the list, if **any** repo is FAIL, STOP
with a single consolidated message naming all failing repos, and
**do not write config, do not proceed to Step 4**:

> ❌ **GitHub MCP connector is missing the following repo(s):**
> - `<repo1>` (error: <short error>)
> - `<repo2>` (error: <short error>)
>
> The cloud schedule will abort every run with
> `cannot post to <repo>`.
>
> **Fix**: open https://claude.ai/settings/connectors, edit your
> GitHub connector, and add the listed repos with **Issues: write**
> and **Pull requests: write** scopes. Then re-run
> `/gardener-onboarding`.

If all repos pass, continue.

### 3.5c. Write-scope verification (best-effort)

Read-only probes confirm the repos are visible but don't prove the
connector can post comments. There is no reliable, safe way to
verify write scope without either calling a scope-introspection
endpoint (which most GitHub MCP connectors don't expose) or
performing a real write (which could leak test comments to the
target repo — **never do this**).

Therefore: **do not attempt any write probe.** Instead, log a
one-line warning and continue:

> ⚠️ Connector read access verified. Couldn't verify write scope
> without risking a real comment — if the first cloud run fails
> with `cannot post to <target_repo>`, re-authorize the connector
> at https://claude.ai/settings/connectors and grant **Issues:
> write** + **Pull requests: write** scopes.

This is the only path through 3.5c. Do not try "dry-run" writes,
empty-body POSTs, or fabricated introspection tools — they either
don't exist or will cause real writes.

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

**External reviewer mode warning**: In external reviewer mode (no label
permission on target), gardener posts a visible comment on **every**
ALIGNED+low item — there is no silent path. On a large repo, the first
fire could post dozens of unsolicited comments to a public repo you
don't maintain.

If `INSTALL_MODE=external`, tell the user:

"⚠️ **First-fire warning for external reviewer mode**: gardener will
post a visible comment on every item it reviews (including ALIGNED
items), because it can't silently label on a repo you don't maintain.

Before the first run on a large repo, I strongly recommend:
1. Set `paths_ignored` in `.claude/gardener-config.yaml` to skip
   docs, tests, vendor, etc.
2. Consider setting `scan_limit: 5` for the first run to test on a
   small batch, then increase after.
3. Run `/gardener-comment-manual` interactively first (not `/gardener-start`)
   so you can see what it's about to post."

Then ask:
"⚠️ The test run will scan open PRs and issues on `<target_repo>` and
may post real comments. Proceed?"

- Yes → execute `.claude/commands/gardener-comment-manual.md` once.
- No → skip to Step 8.

## 7b. Optional: Set up tree sync

Ask the user:
"Would you like to enable automatic tree sync? This detects when
your source repo drifts from the context tree and opens PRs to
update the tree.

Requirements:
- A bound context tree (first-tree must be initialized)
- `first-tree` CLI installed
- `claude` CLI installed and authenticated

Enable tree sync? (y/n)"

If yes:
1. Check that `first-tree` CLI is available: `first-tree --version`
   - If not → "Install first-tree first: npm install -g first-tree"
2. Check that tree has bindings: look for `.first-tree/bindings/*.json`
   - If none → "Bind a source repo first: first-tree bind"
3. Install sync command files:
   - Copy gardener-sync-manual.md, gardener-sync-loop.md,
     gardener-sync-schedule.md, gardener-sync-start.md,
     gardener-sync-stop.md to .claude/commands/
4. Log: "✓ Sync module installed. Start with /gardener-sync-start"

## 8. Confirm

Output:
"🌱 repo-gardener ${GARDENER_VERSION} installed.
- Target repo: `<target_repo>`
- GitHub MCP connector: ✓ verified (cloud commands will work)
- Commands and config committed and pushed to remote.

**Important**: restart Claude Code (or start a new session) so the new
slash commands are picked up. After restarting:

- `/gardener-start` — start all modules (comment + sync)
- `/gardener-comment-manual` — one-off PR review
- `/gardener-sync-manual` — one-off tree sync
- `/gardener-comment-watch` — open a terminal popup that tails run logs live (clickable URLs)
- `/gardener-upgrade` — auto-update to the latest release
- `/gardener-stop` — stop all modules"
