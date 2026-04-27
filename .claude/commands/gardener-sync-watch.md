Open a terminal window that tails the sync run log live.

## 1. Verify the log exists

```bash
LOG="$HOME/.gardener/sync-runs.jsonl"
mkdir -p "$HOME/.gardener"

if [ ! -s "$LOG" ]; then
  echo "⚠️  No sync runs yet — log file is empty or missing."
  echo "    The watcher will still open and wait for the first entry."
  echo "    Run /gardener-sync-manual or wait for the loop/schedule to fire."
  touch "$LOG"
fi
```

## 2. Dedupe — refuse to start if already running

```bash
PIDFILE="$HOME/.gardener/sync-watch.pid"

if [ -f "$PIDFILE" ]; then
  existing_pid=$(cat "$PIDFILE" 2>/dev/null)
  if [ -n "$existing_pid" ] && kill -0 "$existing_pid" 2>/dev/null; then
    echo "⚠️  gardener-sync-watch already running (PID $existing_pid)."
    echo "    To stop it: kill $existing_pid"
    exit 0
  fi
  rm -f "$PIDFILE"
fi
```

## 3. Build the formatter script (always overwrite for updates)

```bash
mkdir -p "$HOME/.gardener"
cat > "$HOME/.gardener/sync-format.sh" <<'SCRIPT'
#!/usr/bin/env bash
# Tail sync-runs.jsonl and format each line as a human-readable block.

LOG="$HOME/.gardener/sync-runs.jsonl"
PIDFILE="$HOME/.gardener/sync-watch.pid"

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
    classify)
      # Per-PR classification event
      local ts source_id pr_number pr_title type proposals_count
      ts=$(echo "$line" | jq -r '.ts // "?"')
      source_id=$(echo "$line" | jq -r '.source_id // "?"')
      pr_number=$(echo "$line" | jq -r '.pr_number // 0')
      pr_title=$(echo "$line" | jq -r '.pr_title // ""')
      type=$(echo "$line" | jq -r '.type // "?"')
      proposals_count=$(echo "$line" | jq -r '.proposals // 0')

      local color icon
      case "$type" in
        TREE_MISS) color="$BLUE"; icon="+" ;;
        TREE_SUPPLEMENT) color="$YELLOW"; icon="~" ;;
        TREE_OK) color="$DIM"; icon="·" ;;
        *) color="$RESET"; icon="?" ;;
      esac

      printf "${DIM}%s${RESET} ${color}%s %s${RESET} ${BOLD}PR #%s${RESET} %s\n" \
        "$ts" "$icon" "$type" "$pr_number" "$pr_title"
      ;;

    apply)
      # PR opened on tree repo
      local ts pr_url pr_title source_pr
      ts=$(echo "$line" | jq -r '.ts // "?"')
      pr_url=$(echo "$line" | jq -r '.pr_url // ""')
      pr_title=$(echo "$line" | jq -r '.pr_title // ""')
      source_pr=$(echo "$line" | jq -r '.source_pr // ""')

      printf "${DIM}%s${RESET} ${GREEN}✓ opened${RESET} " "$ts"
      print_link "$pr_url" "$pr_title"
      printf "\n"
      if [ -n "$source_pr" ]; then
        printf "         ${DIM}from: %s${RESET}\n" "$source_pr"
      fi
      printf "\n"
      ;;

    skip)
      # PR skipped (TREE_OK, already exists, nothing to commit)
      local ts reason pr_number
      ts=$(echo "$line" | jq -r '.ts // "?"')
      reason=$(echo "$line" | jq -r '.reason // "?"')
      pr_number=$(echo "$line" | jq -r '.pr_number // 0')

      printf "${DIM}%s ⏭ PR #%s — %s${RESET}\n" "$ts" "$pr_number" "$reason"
      ;;

    error)
      # Error during sync
      local ts message
      ts=$(echo "$line" | jq -r '.ts // "?"')
      message=$(echo "$line" | jq -r '.message // "?"')

      printf "${RED}%s ✗ %s${RESET}\n" "$ts" "$message"
      ;;

    run)
      # End-of-run summary
      local ts source_id total_prs classified tree_prs_opened duration_s
      ts=$(echo "$line" | jq -r '.ts // "?"')
      source_id=$(echo "$line" | jq -r '.source_id // "?"')
      total_prs=$(echo "$line" | jq -r '.total_prs // 0')
      classified=$(echo "$line" | jq -r '.classified // 0')
      tree_prs_opened=$(echo "$line" | jq -r '.tree_prs_opened // 0')
      duration_s=$(echo "$line" | jq -r '.duration_s // 0')

      local miss=$(echo "$line" | jq -r '.verdicts.TREE_MISS // 0')
      local supplement=$(echo "$line" | jq -r '.verdicts.TREE_SUPPLEMENT // 0')
      local ok=$(echo "$line" | jq -r '.verdicts.TREE_OK // 0')

      printf "${GREEN}╭─ %s ✓ sync complete — %s${RESET}\n" "$ts" "$source_id"
      printf "${GREEN}│${RESET}  %s source PRs → %s classified → %s tree PRs opened\n" \
        "$total_prs" "$classified" "$tree_prs_opened"
      printf "${GREEN}│${RESET}  "
      [ "$miss" -gt 0 ] && printf "${BLUE}MISS %s${RESET}  " "$miss"
      [ "$supplement" -gt 0 ] && printf "${YELLOW}SUPPLEMENT %s${RESET}  " "$supplement"
      [ "$ok" -gt 0 ] && printf "${DIM}OK %s${RESET}  " "$ok"
      printf "\n"
      printf "${GREEN}│${RESET}  ${DIM}%s seconds${RESET}\n" "$duration_s"
      printf "${GREEN}╰─${RESET}\n\n"
      ;;
  esac
}

clear
printf "${BOLD}🌳 gardener-sync — live run log${RESET}\n"
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

chmod +x "$HOME/.gardener/sync-format.sh"
```

## 4. Open a new terminal window running the formatter

```bash
if [[ "$OSTYPE" == darwin* ]]; then
  if osascript -e 'exists application id "com.googlecode.iterm2"' 2>/dev/null | grep -q true; then
    osascript <<APPLESCRIPT
tell application "iTerm"
  activate
  if (count windows) = 0 then
    create window with default profile command "$HOME/.gardener/sync-format.sh"
  else
    tell current window
      create tab with default profile command "$HOME/.gardener/sync-format.sh"
    end tell
  end if
end tell
APPLESCRIPT
  else
    osascript <<APPLESCRIPT
tell application "Terminal"
  activate
  do script "$HOME/.gardener/sync-format.sh"
end tell
APPLESCRIPT
  fi

elif [[ "$OSTYPE" == linux* ]]; then
  if command -v gnome-terminal &>/dev/null; then
    gnome-terminal -- "$HOME/.gardener/sync-format.sh"
  elif command -v konsole &>/dev/null; then
    konsole -e "$HOME/.gardener/sync-format.sh"
  elif command -v xterm &>/dev/null; then
    xterm -e "$HOME/.gardener/sync-format.sh"
  else
    echo "⚠️  No supported terminal emulator found."
    echo "Run manually in a separate window: $HOME/.gardener/sync-format.sh"
  fi

else
  echo "Unknown OS. Run manually: $HOME/.gardener/sync-format.sh"
fi
```

## 5. Confirm

Output:
"🌳 gardener-sync-watch is running in a new terminal window.

- Each source PR classification shows up in real time.
- Tree PRs opened are shown with clickable links.
- Run summaries appear after each sync finishes.
- Stop: Ctrl+C in the watch window, or `kill $(cat ~/.gardener/sync-watch.pid)`.

Note: sync must write to `~/.gardener/sync-runs.jsonl` for this
to show anything. If you see no output, run /gardener-sync-manual
to trigger a sync."
