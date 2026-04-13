# Context Tree Upgrade — v0.2 -> v0.2

## Tree Metadata
- [ ] Confirm the tree-repo skill at `.agents/skills/first-tree/` and `.claude/skills/first-tree/` still resolves correctly after the refresh
- [ ] Replace any stale `context-tree` CLI command references in repo-specific docs, scripts, workflows, or agent config with `first-tree`

## Sync
- [ ] Review .claude/commands/first-tree-sync.md and set up scheduling via /schedule or cron

## Agent Instructions
- [ ] Compare the framework section in `AGENTS.md` with the bundled template (run `first-tree init --help` to see what templates ship) and update the text between the markers if needed
- [ ] Compare the framework section in `CLAUDE.md` with the bundled template and update the text between the markers if needed

## Verification
- [ ] `.first-tree/VERSION` reads `0.2`
- [ ] `first-tree verify` passes

---

**Important:** As you complete each task, check it off in `.first-tree/progress.md` by changing `- [ ]` to `- [x]`. Run `first-tree verify` when done — it will fail if any items remain unchecked.
