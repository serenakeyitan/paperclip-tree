Set up `first-tree gardener` for this Context Tree. Run this from
inside the tree repo (the one with `NODE.md` and `.first-tree/`).

Gardener is a maintenance agent that reviews source-repo PRs and
issues against this tree, responds to reviewer feedback on tree-sync
PRs, and (push mode) installs a workflow that replaces the long-running
gardener service with event-driven per-PR sync.

There are two deployment shapes. Pick the one that matches your repo
access:

| Mode | When to use |
|---|---|
| **Push (workflow)** | You control the source repo and can land a workflow file in it. Lowest latency, zero infra. |
| **Pull (breeze daemon)** | The source repo is third-party or you can't push workflow files. |

## 1. Verify this is a tree repo

```bash
if [ ! -d ".first-tree" ]; then
  echo "❌ This does not look like a Context Tree repo."
  echo "   Run this from inside your tree repo, or create one with:"
  echo "   npx -p first-tree first-tree tree init"
  exit 1
fi
echo "✓ Context Tree detected."
```

## 2. Verify config

Gardener reads `.claude/gardener-config.yaml` from the tree repo:

```yaml
target_repo: owner/app-repo          # source repo to review
tree_repo: owner/tree-repo            # this tree repo (for attribution)
modules:
  comment:
    enabled: true                     # set false to opt out entirely
  respond:
    enabled: true
```

Missing or omitted `modules.<name>.enabled` leaves subcommands
**enabled**. Explicit `enabled: false` is the only opt-out.

If the config is missing, create it from the tree-repo slug and the
bound source repo:

```bash
TREE_REPO=$(gh repo view --json nameWithOwner --jq .nameWithOwner)
# target_repo is the source repo you want gardener to review.
# Look it up from .first-tree/bindings/<source-id>.json if you don't
# know it offhand.
```

## 3. Install a deployment mode

### Push mode (recommended if you control the source repo)

From inside the **source repo** (not the tree repo), run:

```bash
npx -p first-tree first-tree gardener install-workflow \
  --tree-repo <OWNER>/<TREE_REPO_NAME>
```

This scaffolds `.github/workflows/first-tree-sync.yml`. Then set the
`TREE_REPO_TOKEN` secret on the source repo — a PAT with `repo` scope
on the tree repo. See `.agents/skills/first-tree/references/workflow-mode.md`
for the `gh auth token` quick path vs. the scoped-PAT fallback.

Commit the workflow and open a PR. On every PR merge thereafter, the
workflow files a tree-repo issue assigned to the NODE owners.

### Pull mode (breeze daemon)

Install breeze instead (the notification-driven dispatcher):

```bash
npx -p first-tree first-tree breeze install
npx -p first-tree first-tree breeze start
```

breeze polls GitHub notifications and invokes `first-tree gardener
comment` / `respond` on matching items. See the `breeze` skill for
setup, auth, and troubleshooting.

## 4. Confirm

Restart Claude Code so the slash commands appear. You can run
`/gardener-comment` and `/gardener-respond` manually at any time for
a one-off review; the two deployment modes above just automate that
invocation.
