---
title: "Discord Daily Merge Digest"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["infrastructure/ci-cd/NODE.md"]
---

# Discord Daily Merge Digest

`scripts/discord-daily-digest.sh` posts a summary of the day's merges on `master` to a Discord channel via an incoming webhook. The script defaults to today; a scheduled job (cron or CI) typically invokes it once per day and may pass the prior day's date explicitly.

## Configuration

The script requires a `DISCORD_WEBHOOK_URL` environment variable (documented in `.env.example`). It optionally takes a date argument in `YYYY-MM-DD` form; otherwise it defaults to today. Commit enumeration uses `git log --since/--until` against `master` and formats the payload as a Discord embed. Setting `DRY_RUN=1` prints the payload without posting, which is useful when wiring up a new schedule.

## Related

- CI scheduling for this kind of outbound notification belongs under [infrastructure/ci-cd/](../ci-cd/NODE.md). No scheduled workflow has been wired up in-repo yet — today the script must be invoked from an external cron or one-off CI dispatch.
