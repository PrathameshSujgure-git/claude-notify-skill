---
description: Show current notification settings
---

# Notification Status

Display current notification settings.

## Usage

```
/notify:status
```

## Instructions

When this command is invoked:

1. Read `~/.claude/settings.json`
2. Check if notification hooks exist:
   - Look for `hooks.Stop`, `hooks.Notification`, `hooks.PermissionRequest`
3. If hooks exist, parse the current sound from the `-sound` parameter
4. Display status in a formatted table:

```
**Notifications: ✅ Enabled** (or ❌ Disabled)

| Trigger | Sound | Message |
|---------|-------|---------|
| Stop (response done) | [sound] | Response text |
| Notification | [sound] | "Needs your input" |
| PermissionRequest | [sound] | "Permission needed" |

Commands: `/notify:on` · `/notify:off` · `/notify:sound`
```

If disabled, show:
```
**Notifications: ❌ Disabled**

Use `/notify:on` to enable notifications.
```
