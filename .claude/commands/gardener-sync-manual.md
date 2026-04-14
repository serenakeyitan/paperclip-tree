<!-- Context: runs the first-tree sync command as a one-off -->

Run a one-off tree sync. This reads all bound sources from the tree's
`.first-tree/bindings/` directory and runs `first-tree sync --apply`
to detect drift, classify changes, and open tree PRs.

## Prerequisites

- `first-tree` CLI must be installed (`npm install -g first-tree` or `npx first-tree`)
- Tree repo must have `.first-tree/bindings/` with at least one bound source
- `claude` CLI must be on PATH for AI classification
- `gh` CLI must be authenticated

## Run

```bash
first-tree sync --apply --tree-path .
```

If you want to preview without opening PRs:
```bash
first-tree sync --propose --tree-path .
```

If you want to detect drift only:
```bash
first-tree sync --tree-path .
```
