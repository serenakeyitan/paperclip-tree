Open a terminal window that tails the gardener run log live, with
clickable URLs.

## 1. Verify the log exists

Check that `~/.gardener/runs.jsonl` exists. If not, output:
"⚠️ No gardener runs yet. Run `/gardener-manual` once or wait for the
loop/schedule to fire, then re-run `/gardener-watch`."
STOP.

## 2. Dedupe — refuse to start if already running

```bash
PIDFILE="$HOME/.gardener/watch.pid"

if [ -f "$PIDFILE" ]; then
  existing_pid=$(cat "$PIDFILE" 2>/dev/null)
  if [ -n "$existing_pid" ] && kill -0 "$existing_pid" 2>/dev/null; then
    echo "⚠️  gardener-watch already running (PID $existing_pid)."
    echo "    To stop it: kill $existing_pid"
    echo "    Or close the existing terminal window."
    exit 0
  fi
  rm -f "$PIDFILE"
fi
```

## 3. Build the formatter script (one-time, idempotent)

Always overwrite `$HOME/.gardener/format.sh` so updates land:

```bash
mkdir -p "$HOME/.gardener"
cat > "$HOME/.gardener/format.sh" <<'SCRIPT'
#!/usr/bin/env bash
# Tail gardener runs.jsonl and format each line as a human-readable
# block with OSC 8 hyperlinks (clickable in modern terminals).

LOG="$HOME/.gardener/runs.jsonl"
PIDFILE="$HOME/.gardener/watch.pid"

# Write our PID and clean up on exit
echo $$ > "$PIDFILE"
trap 'rm -f "$PIDFILE"; exit 0' INT TERM EXIT

# OSC 8 hyperlink: $1=url $2=label
hyperlink() {
  printf '\e]8;;%s\e\\%s\e]8;;\e\\' "$1" "$2"
}

format_run() {
  local line="$1"

  # Skip malformed lines silently (rotation, partial writes, etc.)
  echo "$line" | jq empty 2>/dev/null || return 0

  local ts mode target reviewed scanned_prs scanned_issues errors
  local aligned new needs conflict insuf

  ts=$(echo "$line" | jq -r '.ts // "?"')
  mode=$(echo "$line" | jq -r '.mode // "?"')
  target=$(echo "$line" | jq -r '.target_repo // "?"')
  reviewed=$(echo "$line" | jq -r '.reviewed // 0')
  scanned_prs=$(echo "$line" | jq -r '.scanned_prs // 0')
  scanned_issues=$(echo "$line" | jq -r '.scanned_issues // 0')
  errors=$(echo "$line" | jq -r '.errors | length // 0')
  aligned=$(echo "$line" | jq -r '.verdicts.ALIGNED // 0')
  new=$(echo "$line" | jq -r '.verdicts.NEW_TERRITORY // 0')
  needs=$(echo "$line" | jq -r '.verdicts.NEEDS_REVIEW // 0')
  conflict=$(echo "$line" | jq -r '.verdicts.CONFLICT // 0')
  insuf=$(echo "$line" | jq -r '.verdicts.INSUFFICIENT_CONTEXT // 0')

  # Default any non-numeric values to 0 for safe arithmetic
  [[ "$reviewed" =~ ^[0-9]+$ ]] || reviewed=0
  [[ "$errors" =~ ^[0-9]+$ ]] || errors=0
  [[ "$conflict" =~ ^[0-9]+$ ]] || conflict=0
  [[ "$needs" =~ ^[0-9]+$ ]] || needs=0

  # Color code by status
  local color icon reset
  reset="\033[0m"
  if [ "$errors" -gt 0 ]; then
    color="\033[31m"  # red
    icon="✗"
  elif [ "$conflict" -gt 0 ] || [ "$needs" -gt 0 ]; then
    color="\033[33m"  # yellow
    icon="⚠"
  elif [ "$reviewed" -gt 0 ]; then
    color="\033[32m"  # green
    icon="✓"
  else
    color="\033[2m"   # dim
    icon="·"
  fi

  printf "${color}%s %s${reset}  %-9s  " "$ts" "$icon" "$mode"
  hyperlink "https://github.com/$target" "$target"
  printf "\n"
  printf "         scanned %s PRs / %s issues · reviewed %s\n" "$scanned_prs" "$scanned_issues" "$reviewed"

  if [ "$reviewed" -gt 0 ]; then
    printf "         "
    [ "$aligned" -gt 0 ] && printf "ALIGNED %s  " "$aligned"
    [ "$new" -gt 0 ] && printf "NEW %s  " "$new"
    [ "$needs" -gt 0 ] && printf "NEEDS %s  " "$needs"
    [ "$conflict" -gt 0 ] && printf "\033[33mCONFLICT %s\033[0m  " "$conflict"
    [ "$insuf" -gt 0 ] && printf "INSUFFICIENT %s  " "$insuf"
    printf "\n"
  fi

  # Per-item lines with clickable URL via OSC 8 (jq emits ESC bytes directly)
  echo "$line" | jq -r '.items[]? | "         \(.verdict) #\(.number) \u001B]8;;\(.url)\u001B\\\(.title)\u001B]8;;\u001B\\"' 2>/dev/null

  if [ "$errors" -gt 0 ]; then
    echo "$line" | jq -r '.errors[]? | "         \u001B[31m✗ \(.)\u001B[0m"' 2>/dev/null
  fi
  printf "\n"
}

clear
echo "🌱 repo-gardener — live run log"
echo "Stop: Ctrl+C in this window"
echo "─────────────────────────────────────────────────────────"
echo

# Replay last 20 lines, then follow new ones
tail -n 20 -F "$LOG" 2>/dev/null | while IFS= read -r line; do
  format_run "$line"
done
SCRIPT

chmod +x "$HOME/.gardener/format.sh"
```

## 4. Open a new terminal window running the formatter

Use `$HOME` (not `~`) in `osascript` strings — AppleScript does not
expand `~` for non-shell `command` parameters.

**macOS Terminal.app** (default — try this first):

```bash
if [[ "$OSTYPE" == darwin* ]]; then
  osascript <<APPLESCRIPT
tell application "Terminal"
  do script "$HOME/.gardener/format.sh"
  activate
end tell
APPLESCRIPT
fi
```

**Linux**: try in order, use the first that exists:

```bash
if [[ "$OSTYPE" != darwin* ]]; then
  if command -v gnome-terminal &>/dev/null; then
    gnome-terminal -- "$HOME/.gardener/format.sh"
  elif command -v konsole &>/dev/null; then
    konsole -e "$HOME/.gardener/format.sh"
  elif command -v xterm &>/dev/null; then
    xterm -e "$HOME/.gardener/format.sh"
  else
    echo "⚠️  No supported terminal emulator found."
    echo "Run manually: $HOME/.gardener/format.sh"
    echo "(This will block the current session — Ctrl+C to exit.)"
  fi
fi
```

**SSH session / no display**: spawning a new terminal won't work over
SSH. Detect and fall back to inline:

```bash
if [ -n "$SSH_CONNECTION" ] || [ -z "$DISPLAY" ] && [[ "$OSTYPE" != darwin* ]]; then
  echo "⚠️  Running in SSH or headless session."
  echo "To watch the log here (will block until Ctrl+C):"
  echo "    $HOME/.gardener/format.sh"
  echo
  echo "To watch from your local machine instead, run /gardener-watch there."
fi
```

## 5. Confirm

Output:
"🌱 gardener-watch is running in a new terminal window.

- Repo links and item titles are clickable (⌘-click on macOS,
  Ctrl-click on Linux) in modern terminals.
- Color codes: green = clean, yellow = conflicts/needs-review,
  red = errors, dim = no work.
- Stop: Ctrl+C in the watch window, or `kill $(cat ~/.gardener/watch.pid)`.
- Re-running `/gardener-watch` while one is already active is a no-op
  (refuses to start a duplicate)."
