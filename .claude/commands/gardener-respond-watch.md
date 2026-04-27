Open a terminal window that tails the respond run log live.

## 1. Verify the log exists

```bash
LOG="$HOME/.gardener/respond-runs.jsonl"
mkdir -p "$HOME/.gardener"

if [ ! -s "$LOG" ]; then
  echo "⚠️  No respond runs yet — log file is empty or missing."
  echo "    The watcher will still open and wait for the first entry."
  echo "    Run /gardener-respond-manual or wait for the loop/schedule to fire."
  touch "$LOG"
fi
```

## 2. Dedupe — refuse to start if already running

```bash
PIDFILE="$HOME/.gardener/respond-watch.pid"

if [ -f "$PIDFILE" ]; then
  existing_pid=$(cat "$PIDFILE" 2>/dev/null)
  if [ -n "$existing_pid" ] && kill -0 "$existing_pid" 2>/dev/null; then
    echo "⚠️  gardener-respond-watch already running (PID $existing_pid)."
    echo "    To stop it: kill $existing_pid"
    exit 0
  fi
  rm -f "$PIDFILE"
fi
```

## 3. Build the formatter script (always overwrite for updates)

```bash
mkdir -p "$HOME/.gardener"
cat > "$HOME/.gardener/respond-format.sh" <<'SCRIPT'
#!/usr/bin/env bash
# Tail respond-runs.jsonl and format each line as a human-readable block.

LOG="$HOME/.gardener/respond-runs.jsonl"
PIDFILE="$HOME/.gardener/respond-watch.pid"

# Write PID and clean up on exit
echo $$ > "$PIDFILE"
trap 'rm -f "$PIDFILE"; exit 0' INT TERM EXIT

# Real ESC byte
ESC=$(printf '\033')
RESET="${ESC}[0m"
DIM="${ESC}[2m"
RED="${ESC}[31m"
GREEN="${ESC}[32m"
YELLOW="${ESC}[33m"
BLUE="${ESC}[34m"
CYAN="${ESC}[36m"
BOLD="${ESC}[1m"

# Detect OSC 8 support
SUPPORTS_HYPERLINKS=false
case "$TERM_PROGRAM" in
  iTerm.app|WezTerm|vscode) SUPPORTS_HYPERLINKS=true ;;
esac
[ -n "$KITTY_WINDOW_ID" ] && SUPPORTS_HYPERLINKS=true
[ -n "$WT_SESSION" ] && SUPPORTS_HYPERLINKS=true

print_link() {
  local url="$1"
  local label="$2"
  if [ "$SUPPORTS_HYPERLINKS" = "true" ]; then
    printf "%s]8;;%s%s\\\\%s%s]8;;%s\\\\" "$ESC" "$url" "$ESC" "$label" "$ESC" "$ESC"
  else
    printf "%s %s%s%s" "$label" "$DIM" "$url" "$RESET"
  fi
}

format_line() {
  local line="$1"

  # Skip malformed lines silently
  echo "$line" | jq empty 2>/dev/null || return 0

  local kind=$(echo "$line" | jq -r '.kind // "run"')

  case "$kind" in
    scan)
      # Scan result: how many sync PRs found, how many need fixes
      local ts tree_repo total_prs needs_fix
      ts=$(echo "$line" | jq -r '.ts // "?"')
      tree_repo=$(echo "$line" | jq -r '.tree_repo // "?"')
      total_prs=$(echo "$line" | jq -r '.total_prs // 0')
      needs_fix=$(echo "$line" | jq -r '.needs_fix // 0')

      printf "${DIM}%s${RESET} ${YELLOW}⬤ scan${RESET} %s — %s PRs found, %s need fixes\n" \
        "$ts" "$tree_repo" "$total_prs" "$needs_fix"
      ;;

    fix)
      # A PR was fixed
      local ts pr_number pattern summary
      ts=$(echo "$line" | jq -r '.ts // "?"')
      pr_number=$(echo "$line" | jq -r '.pr_number // 0')
      pattern=$(echo "$line" | jq -r '.pattern // "?"')
      summary=$(echo "$line" | jq -r '.summary // ""')

      printf "${DIM}%s${RESET} ${GREEN}✓ fix${RESET} ${BOLD}PR #%s${RESET} [%s] %s\n" \
        "$ts" "$pr_number" "$pattern" "$summary"
      ;;

    reply)
      # Reply posted to a reviewer
      local ts pr_number reviewer
      ts=$(echo "$line" | jq -r '.ts // "?"')
      pr_number=$(echo "$line" | jq -r '.pr_number // 0')
      reviewer=$(echo "$line" | jq -r '.reviewer // "?"')

      printf "${DIM}%s${RESET} ${CYAN}💬 reply${RESET} ${BOLD}PR #%s${RESET} → @%s\n" \
        "$ts" "$pr_number" "$reviewer"
      ;;

    learn)
      # Pattern extracted from review feedback
      local ts pattern count
      ts=$(echo "$line" | jq -r '.ts // "?"')
      pattern=$(echo "$line" | jq -r '.pattern // "?"')
      count=$(echo "$line" | jq -r '.count // 1')

      printf "${DIM}%s${RESET} ${BLUE}📘 learn${RESET} pattern=%s (count: %s)\n" \
        "$ts" "$pattern" "$count"
      ;;

    skip)
      # PR skipped (already fixed, no review, etc.)
      local ts pr_number reason
      ts=$(echo "$line" | jq -r '.ts // "?"')
      pr_number=$(echo "$line" | jq -r '.pr_number // 0')
      reason=$(echo "$line" | jq -r '.reason // "?"')

      printf "${DIM}%s ⏭ PR #%s — %s${RESET}\n" "$ts" "$pr_number" "$reason"
      ;;

    error)
      # Error during respond
      local ts message
      ts=$(echo "$line" | jq -r '.ts // "?"')
      message=$(echo "$line" | jq -r '.message // "?"')

      printf "${RED}%s ✗ %s${RESET}\n" "$ts" "$message"
      ;;

    run)
      # End-of-run summary
      local ts prs_scanned prs_fixed learnings duration_s
      ts=$(echo "$line" | jq -r '.ts // "?"')
      prs_scanned=$(echo "$line" | jq -r '.prs_scanned // 0')
      prs_fixed=$(echo "$line" | jq -r '.prs_fixed // 0')
      learnings=$(echo "$line" | jq -r '.learnings // 0')
      duration_s=$(echo "$line" | jq -r '.duration_s // 0')

      printf "${GREEN}╭─ %s ✓ respond complete${RESET}\n" "$ts"
      printf "${GREEN}│${RESET}  PRs scanned: %s — fixed: %s — learnings: %s\n" \
        "$prs_scanned" "$prs_fixed" "$learnings"
      printf "${GREEN}│${RESET}  ${DIM}%s seconds${RESET}\n" "$duration_s"
      printf "${GREEN}╰─${RESET}\n\n"
      ;;
  esac
}

clear
printf "${BOLD}🔧 gardener-respond — live run log${RESET}\n"
printf "${DIM}Stop: Ctrl+C in this window${RESET}\n"
if [ "$SUPPORTS_HYPERLINKS" = "true" ]; then
  printf "${DIM}Links are clickable (⌘-click on macOS)${RESET}\n"
fi
printf "${DIM}─────────────────────────────────────────────────────────${RESET}\n\n"

# Replay last 30 lines, then follow new ones
tail -n 30 -F "$LOG" 2>/dev/null | while IFS= read -r line; do
  format_line "$line"
done
SCRIPT

chmod +x "$HOME/.gardener/respond-format.sh"
```

## 4. Open a new terminal window running the formatter

```bash
if [[ "$OSTYPE" == darwin* ]]; then
  if osascript -e 'exists application id "com.googlecode.iterm2"' 2>/dev/null | grep -q true; then
    osascript <<APPLESCRIPT
tell application "iTerm"
  activate
  if (count windows) = 0 then
    create window with default profile command "$HOME/.gardener/respond-format.sh"
  else
    tell current window
      create tab with default profile command "$HOME/.gardener/respond-format.sh"
    end tell
  end if
end tell
APPLESCRIPT
  else
    osascript <<APPLESCRIPT
tell application "Terminal"
  activate
  do script "$HOME/.gardener/respond-format.sh"
end tell
APPLESCRIPT
  fi

elif [[ "$OSTYPE" == linux* ]]; then
  if command -v gnome-terminal &>/dev/null; then
    gnome-terminal -- "$HOME/.gardener/respond-format.sh"
  elif command -v konsole &>/dev/null; then
    konsole -e "$HOME/.gardener/respond-format.sh"
  elif command -v xterm &>/dev/null; then
    xterm -e "$HOME/.gardener/respond-format.sh"
  else
    echo "⚠️  No supported terminal emulator found."
    echo "Run manually in a separate window: $HOME/.gardener/respond-format.sh"
  fi

else
  echo "Unknown OS. Run manually: $HOME/.gardener/respond-format.sh"
fi
```

## 5. Confirm

Output:
"🔧 gardener-respond-watch is running in a new terminal window.

- Each scan shows PRs found and how many need fixes.
- Fixes show the PR number, pattern type, and summary.
- Replies to reviewers appear with the reviewer's username.
- Learnings extracted are shown with pattern name and count.
- Run summaries appear after each respond cycle finishes.
- Stop: Ctrl+C in the watch window, or `kill $(cat ~/.gardener/respond-watch.pid)`.

Note: respond must write to `~/.gardener/respond-runs.jsonl` for this
to show anything. If you see no output, run /gardener-respond-manual
to trigger a respond cycle."
