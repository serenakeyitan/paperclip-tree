# Context Tree Upgrade — v0.2 -> v0.3

## Tree Metadata
- [x] Confirm the tree-repo skill at `.agents/skills/first-tree/` and `.claude/skills/first-tree/` still resolves correctly after the refresh
- [x] Replace any stale `context-tree` CLI command references in repo-specific docs, scripts, workflows, or agent config with `first-tree`

## Sync
- [x] Review .claude/commands/first-tree-sync.md and set up scheduling via /schedule or cron

## Agent Instructions
- [x] Compare the framework section in `AGENTS.md` with the bundled template (run `first-tree tree init --help` to see what templates ship) and update the text between the markers if needed
- [x] Compare the framework section in `CLAUDE.md` with the bundled template and update the text between the markers if needed

## Verification
- [x] `.first-tree/VERSION` reads `0.3`
- [x] `first-tree tree verify` passes

---

**Important:** As you complete each task, check it off in `.first-tree/progress.md` by changing `- [x]` to `- [x]`. Run `first-tree tree verify` when done — it will fail if any items remain unchecked.
