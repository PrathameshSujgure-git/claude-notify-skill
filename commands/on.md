---
description: Enable macOS notifications for Claude Code
---

# Enable Notifications

Turn on macOS notifications for task completion, permission requests, and input needed.

## Usage

```
/notify:on
```

## Instructions

When this command is invoked:

1. Read `~/.claude/settings.json`
2. Add or merge the following hooks:

```json
{
  "hooks": {
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "input=$(cat); transcript=$(echo \"$input\" | jq -r '.transcript_path'); msg=$(tail -20 \"$transcript\" | grep '\"type\":\"assistant\"' | tail -1 | jq -r '.message.content[] | select(.type==\"text\") | .text' 2>/dev/null | head -c 200); [ -z \"$msg\" ] && msg='Task finished'; terminal-notifier -title 'Claude Code' -message \"$msg\" -sound Pop -activate com.apple.Terminal"
          }
        ]
      }
    ],
    "Notification": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "terminal-notifier -title 'Claude Code' -message 'Needs your input' -sound Ping -activate com.apple.Terminal"
          }
        ]
      }
    ],
    "PermissionRequest": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "terminal-notifier -title 'Claude Code' -message 'Permission needed' -sound Ping -activate com.apple.Terminal"
          }
        ]
      }
    ]
  }
}
```

3. Write the updated settings.json
4. Inform the user: "Notifications enabled! Restart Claude Code for changes to take effect."

## Requirements

- macOS
- `terminal-notifier` (install via `brew install terminal-notifier`)
- `jq` (install via `brew install jq`)
