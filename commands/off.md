---
description: Disable macOS notifications for Claude Code
---

# Disable Notifications

Turn off macOS notifications.

## Usage

```
/notify:off
```

## Instructions

When this command is invoked:

1. Read `~/.claude/settings.json`
2. Remove the following hooks if they exist:
   - `Stop`
   - `Notification`
   - `PermissionRequest`
3. If the `hooks` object is empty after removal, remove it entirely
4. Write the updated settings.json
5. Inform the user: "Notifications disabled! Restart Claude Code for changes to take effect."
