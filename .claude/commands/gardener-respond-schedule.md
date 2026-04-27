<!-- Context: runs under /schedule in the Anthropic cloud -->

**UNATTENDED=true** — do not prompt the user.

**RUN_MODE=schedule**

## What this does

Runs gardener-respond in cloud mode. All GitHub access goes through
MCP tools instead of `gh` CLI.

Read and execute `.claude/commands/gardener-respond-manual.md` as a
runbook. When the manual says to use `gh` commands, use the
equivalent MCP tool instead:

| `gh` command | MCP equivalent |
|-------------|----------------|
| `gh pr list` | `mcp__github__list_pull_requests` |
| `gh pr merge` | `mcp__github__merge_pull_request` |
| `gh pr comment` | `mcp__github__create_issue_comment` |
| `gh pr close` | `mcp__github__update_pull_request` (state=closed) |
| `gh api repos/.../pulls/.../reviews` | `mcp__github__list_pull_request_reviews` or raw API |
| `gh api repos/.../pulls/.../comments` | `mcp__github__list_pull_request_comments` |

For git operations (checkout, commit, push), use
`mcp__github__create_or_update_file` to write fixes directly to the
PR branch without a local checkout.
