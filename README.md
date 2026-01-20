# Claude Code Notify

Get macOS notifications when Claude Code finishes tasks, needs input, or requests permissions.

## Features

- **Task Completion** - Get notified with the actual response text when Claude finishes
- **Permission Requests** - Know when Claude needs approval for file edits, commands, etc.
- **Input Needed** - Alert when Claude asks a question or needs your input
- **Click to Focus** - Clicking the notification brings Terminal to the foreground
- **Customizable Sounds** - Choose from Pop, Glass, Ping, Funk, Basso, and more

## Requirements

- macOS (see [Windows/Linux alternatives](#windowslinux-alternatives) below)
- [Claude Code CLI](https://docs.anthropic.com/en/docs/claude-code)
- `terminal-notifier` and `jq`:
  ```bash
  brew install terminal-notifier jq
  ```

## Installation

### Option 1: Via Claude Code Commands (Recommended)

1. Add the marketplace:
   ```
   /plugin marketplace add PrathameshSujgure-git/claude-notify-skill
   ```

2. Install the plugin:
   ```
   /plugin install notify@notify-plugins
   ```

3. Restart Claude Code

4. Use `/notify:on` to enable notifications

### Option 2: Manual Setup (Copy & Paste)

Add this to your `~/.claude/settings.json`:

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

## Commands

| Command | Description |
|---------|-------------|
| `/notify:on` | Enable notifications |
| `/notify:off` | Disable notifications |
| `/notify:sound` | Change notification sound |
| `/notify:status` | Show current settings |

## Configuration

### Enable Notification Previews

To see the response text in notifications:

1. Open **System Settings** → **Notifications**
2. Find **terminal-notifier** in the list
3. Set **Show Previews** → **Always**

### Change Notification Sound

Use `/notify:sound` or manually edit the `-sound` parameter:

| Sound | Description |
|-------|-------------|
| Pop | Short, subtle (default) |
| Glass | Clear glass tap |
| Ping | Soft ping |
| Funk | Funky alert |
| Basso | Deep bass note |

### Change Terminal App

Update the `-activate` parameter in the hooks:

| Terminal | Bundle ID |
|----------|-----------|
| Terminal.app | `com.apple.Terminal` |
| iTerm2 | `com.googlecode.iterm2` |
| Warp | `dev.warp.Warp-Stable` |
| VS Code | `com.microsoft.VSCode` |
| Kitty | `net.kovidgoyal.kitty` |

## Hook Events

| Event | When | Default Message |
|-------|------|-----------------|
| `Stop` | Claude finishes responding | Actual response text |
| `Notification` | Claude needs input | "Needs your input" |
| `PermissionRequest` | Claude asks permission | "Permission needed" |

## Windows/Linux Alternatives

### Windows (PowerShell + BurntToast)

```powershell
Install-Module -Name BurntToast
```

Replace hook command with:
```json
"command": "powershell -Command \"New-BurntToastNotification -Text 'Claude Code', 'Task finished'\""
```

### Linux (notify-send)

```bash
sudo apt install libnotify-bin
```

Replace hook command with:
```json
"command": "notify-send 'Claude Code' 'Task finished'"
```

## Troubleshooting

### Notifications not appearing
1. Check installation: `which terminal-notifier`
2. Test: `terminal-notifier -title "Test" -message "Hello"`
3. Check System Settings → Notifications

### No preview text
Enable previews: System Settings → Notifications → terminal-notifier → Show Previews → Always

### Changes not working
Restart Claude Code after modifying settings.

## License

MIT

## Credits

Built with [Claude Code](https://docs.anthropic.com/en/docs/claude-code) by Anthropic.
