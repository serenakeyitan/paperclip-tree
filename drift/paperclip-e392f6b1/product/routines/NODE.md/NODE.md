---
title: "Routines"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Routines

Scheduled or event-triggered automation definitions scoped to a company. Routines enable recurring or reactive workflows without manual intervention.

## Key Decisions

### Draft Lifecycle

Routines follow a draft → published lifecycle. A routine must be explicitly published before it can trigger. This prevents accidental execution of incomplete automation.

### Trigger Kinds

Multiple trigger types are supported: cron (time-based), event (reactive), and manual. Each routine declares its trigger kind at creation.

### Concurrency Policies

When a routine triggers while a previous run is still active, behavior is controlled by a concurrency policy: skip (ignore the new trigger), queue (run after current completes), or cancel-previous (abort the running instance and start fresh).

### Portability

Routines are included in the `.paperclip.yaml` sidecar file, making them portable across Paperclip instances. Run-time variable overrides allow environment-specific configuration without modifying the routine definition.
