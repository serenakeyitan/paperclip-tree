Open a terminal window that tails the gardener run log live.

## 1. Verify the log exists

```bash
LOG="$HOME/.gardener/runs.jsonl"
mkdir -p "$HOME/.gardener"

if [ ! -s "$LOG" ]; then
  echo "⚠️  No gardener runs yet — log file is empty or missing."
  echo "    The watcher will still open and wait for the first entry."
  echo "    Run /gardener-manual or wait for the loop/schedule to fire."
  touch "$LOG"
fi
```

## 2. Dedupe — refuse to start if already running

```bash
PIDFILE="$HOME/.gardener/watch.pid"

if [ -f "$PIDFILE" ]; then
  existing_pid=$(cat "$PIDFILE" 2>/dev/null)
  if [ -n "$existing_pid" ] && kill -0 "$existing_pid" 2>/dev/null; then
    echo "⚠️  gardener-watch already running (PID $existing_pid)."
    echo "    To stop it: kill $existing_pid"
    exit 0
  fi
  rm -f "$PIDFILE"
fi
```

## 3. Build the formatter script (always overwrite for updates)

```bash
mkdir -p "$HOME/.gardener"
cat > "$HOME/.gardener/format.sh" <<'SCRIPT'
#!/usr/bin/env bash
# Tail gardener runs.jsonl and format each line as a human-readable
# block. Auto-detects terminal hyperlink support (OSC 8) and falls
# back to plain URLs on a separate line for terminals that don't
# support it (e.g. macOS Terminal.app).

LOG="$HOME/.gardener/runs.jsonl"
PIDFILE="$HOME/.gardener/watch.pid"

# Write PID and clean up on exit
echo $$ > "$PIDFILE"
trap 'rm -f "$PIDFILE"; exit 0' INT TERM EXIT

# Real ESC byte (bash 3.2 / BSD printf compatible — \e is not portable)
ESC=$(printf '\033')
RESET="${ESC}[0m"
DIM="${ESC}[2m"
RED="${ESC}[31m"
GREEN="${ESC}[32m"
YELLOW="${ESC}[33m"
BLUE="${ESC}[34m"
CYAN="${ESC}[36m"
BOLD="${ESC}[1m"

# Detect OSC 8 support. macOS Terminal.app does NOT support it.
# iTerm2, kitty, alacritty, VS Code, WezTerm, gnome-terminal do.
SUPPORTS_HYPERLINKS=false
case "$TERM_PROGRAM" in
  iTerm.app|WezTerm|vscode) SUPPORTS_HYPERLINKS=true ;;
esac
[ -n "$KITTY_WINDOW_ID" ] && SUPPORTS_HYPERLINKS=true
[ -n "$WT_SESSION" ] && SUPPORTS_HYPERLINKS=true
# Apple_Terminal (Terminal.app) → false, no OSC 8

# Print a hyperlink if supported, otherwise print "label (url)"
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
    item)
      # Per-item event posted while gardener is processing
      local ts verdict severity number type url title target
      ts=$(echo "$line" | jq -r '.ts // "?"')
      verdict=$(echo "$line" | jq -r '.verdict // "?"')
      severity=$(echo "$line" | jq -r '.severity // "?"')
      number=$(echo "$line" | jq -r '.number // 0')
      type=$(echo "$line" | jq -r '.type // "?"')
      url=$(echo "$line" | jq -r '.url // ""')
      title=$(echo "$line" | jq -r '.title // ""')
      target=$(echo "$line" | jq -r '.target_repo // ""')

      local color
      case "$verdict" in
        ALIGNED) color="$GREEN" ;;
        NEW_TERRITORY) color="$BLUE" ;;
        NEEDS_REVIEW) color="$YELLOW" ;;
        CONFLICT) color="$RED" ;;
        INSUFFICIENT_CONTEXT) color="$DIM" ;;
        *) color="$RESET" ;;
      esac

      printf "${DIM}%s${RESET} ${color}● %s${RESET} ${BOLD}%s #%s${RESET}\n" \
        "$ts" "$verdict" "$type" "$number"
      printf "         "
      print_link "$url" "$title"
      printf "\n\n"
      ;;
    run)
      # End-of-run summary
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

      [[ "$reviewed" =~ ^[0-9]+$ ]] || reviewed=0
      [[ "$errors" =~ ^[0-9]+$ ]] || errors=0
      [[ "$conflict" =~ ^[0-9]+$ ]] || conflict=0
      [[ "$needs" =~ ^[0-9]+$ ]] || needs=0

      local color icon
      if [ "$errors" -gt 0 ]; then
        color="$RED"; icon="✗"
      elif [ "$conflict" -gt 0 ] || [ "$needs" -gt 0 ]; then
        color="$YELLOW"; icon="⚠"
      elif [ "$reviewed" -gt 0 ]; then
        color="$GREEN"; icon="✓"
      else
        color="$DIM"; icon="·"
      fi

      printf "${color}╭─ %s %s — %s run on " "$ts" "$icon" "$mode"
      print_link "https://github.com/$target" "$target"
      printf "${RESET}\n"
      printf "${color}│${RESET}  scanned %s PRs / %s issues · reviewed %s\n" \
        "$scanned_prs" "$scanned_issues" "$reviewed"

      if [ "$reviewed" -gt 0 ]; then
        printf "${color}│${RESET}  "
        [ "$aligned" -gt 0 ] && printf "${GREEN}ALIGNED %s${RESET}  " "$aligned"
        [ "$new" -gt 0 ] && printf "${BLUE}NEW %s${RESET}  " "$new"
        [ "$needs" -gt 0 ] && printf "${YELLOW}NEEDS %s${RESET}  " "$needs"
        [ "$conflict" -gt 0 ] && printf "${RED}CONFLICT %s${RESET}  " "$conflict"
        [ "$insuf" -gt 0 ] && printf "${DIM}INSUFFICIENT %s${RESET}  " "$insuf"
        printf "\n"
      fi

      if [ "$errors" -gt 0 ]; then
        echo "$line" | jq -r '.errors[]? | "         \u001B[31m✗ \(.)\u001B[0m"' 2>/dev/null
      fi
      printf "${color}╰─${RESET}\n\n"
      ;;
  esac
}

clear
printf "${BOLD}🌱 repo-gardener — live run log${RESET}\n"
printf "${DIM}Stop: Ctrl+C in this window${RESET}\n"
if [ "$SUPPORTS_HYPERLINKS" = "true" ]; then
  printf "${DIM}Links are clickable in this terminal (⌘-click on macOS, Ctrl-click on Linux)${RESET}\n"
else
  printf "${DIM}Note: this terminal does not support inline hyperlinks. URLs are shown after the title.${RESET}\n"
  printf "${DIM}For clickable links, use iTerm2, VS Code, or kitty.${RESET}\n"
fi
printf "${DIM}─────────────────────────────────────────────────────────${RESET}\n\n"

# Replay last 30 lines, then follow new ones
tail -n 30 -F "$LOG" 2>/dev/null | while IFS= read -r line; do
  format_line "$line"
done
SCRIPT

chmod +x "$HOME/.gardener/format.sh"
```

## 4. Open a new terminal window running the formatter

Prefer iTerm2 if installed (supports clickable hyperlinks), otherwise
fall back to Terminal.app.

```bash
if [[ "$OSTYPE" == darwin* ]]; then
  # Try iTerm2 first (better hyperlink support)
  if osascript -e 'exists application id "com.googlecode.iterm2"' 2>/dev/null | grep -q true; then
    osascript <<APPLESCRIPT
tell application "iTerm"
  activate
  if (count windows) = 0 then
    create window with default profile command "$HOME/.gardener/format.sh"
  else
    tell current window
      create tab with default profile command "$HOME/.gardener/format.sh"
    end tell
  end if
end tell
APPLESCRIPT
  else
    osascript <<APPLESCRIPT
tell application "Terminal"
  activate
  do script "$HOME/.gardener/format.sh"
end tell
APPLESCRIPT
  fi

elif [[ "$OSTYPE" == linux* ]]; then
  if command -v gnome-terminal &>/dev/null; then
    gnome-terminal -- "$HOME/.gardener/format.sh"
  elif command -v konsole &>/dev/null; then
    konsole -e "$HOME/.gardener/format.sh"
  elif command -v xterm &>/dev/null; then
    xterm -e "$HOME/.gardener/format.sh"
  else
    echo "⚠️  No supported terminal emulator found."
    echo "Run manually in a separate window: $HOME/.gardener/format.sh"
  fi

else
  echo "Unknown OS. Run manually: $HOME/.gardener/format.sh"
fi
```

## 5. Confirm

Output:
"🌱 gardener-watch is running in a new terminal window.

- Each PR/issue gardener processes shows up in real time.
- Run summaries appear after each batch finishes.
- Stop: Ctrl+C in the watch window, or `kill $(cat ~/.gardener/watch.pid)`.

If the URLs aren't clickable in your terminal, install iTerm2, kitty,
or VS Code's integrated terminal — Apple's Terminal.app does not
support inline hyperlinks. Your terminal type was detected as
`$TERM_PROGRAM`."
