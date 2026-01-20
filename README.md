# Claude Code Notify

Get macOS notifications when Claude Code finishes tasks, needs input, or requests permissions.

![Demo](./demo.gif)

## Features

- **Task Completion** - Get notified with the actual response text when Claude finishes
- **Permission Requests** - Know when Claude needs approval for file edits, commands, etc.
- **Input Needed** - Alert when Claude asks a question or needs your input
- **Click to Focus** - Clicking the notification brings Terminal to the foreground
- **Customizable Sounds** - Choose from Pop, Glass, Ping, Funk, or Basso

## Requirements

- macOS (see [Windows/Linux alternatives](#windowslinux-alternatives) below)
- [Claude Code CLI](https://claude.ai/download)
- `terminal-notifier` - Install via Homebrew:
  ```bash
  brew install terminal-notifier
  ```
- `jq` - Install via Homebrew:
  ```bash
  brew install jq
  ```

## Installation

### Option 1: Quick Setup (Copy & Paste)

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

### Option 2: Merge with Existing Settings

If you already have a `settings.json`, merge the `hooks` object with your existing configuration.

## Configuration

### Enable Notification Previews

To see the response text in notifications:

1. Open **System Settings** → **Notifications**
2. Find **terminal-notifier** in the list
3. Set **Show Previews** → **Always**

### Change Notification Sound

Edit the `-sound` parameter in the hook commands. Available sounds:
- `Pop` (default)
- `Glass`
- `Ping`
- `Funk`
- `Basso`
- `Blow`
- `Bottle`
- `Frog`
- `Hero`
- `Morse`
- `Purr`
- `Sosumi`
- `Submarine`
- `Tink`

### Change Terminal App

If you use a different terminal (iTerm2, Warp, etc.), update the `-activate` parameter:

| Terminal | Bundle ID |
|----------|-----------|
| Terminal.app | `com.apple.Terminal` |
| iTerm2 | `com.googlecode.iterm2` |
| Warp | `dev.warp.Warp-Stable` |
| VS Code | `com.microsoft.VSCode` |
| Kitty | `net.kovidgoyal.kitty` |
| Alacritty | `org.alacritty` |

## Hook Events Explained

| Event | When it Fires | Default Message |
|-------|---------------|-----------------|
| `Stop` | Claude finishes responding | Last response text |
| `Notification` | Claude needs general input | "Needs your input" |
| `PermissionRequest` | Claude asks for permission (edit file, run command) | "Permission needed" |

## Windows/Linux Alternatives

This skill uses `terminal-notifier` which is macOS only. For other platforms:

### Windows (PowerShell + BurntToast)

```powershell
# Install BurntToast
Install-Module -Name BurntToast

# Notification command
New-BurntToastNotification -Text "Claude Code", "Task finished"
```

Replace the `command` in hooks with:
```json
"command": "powershell -Command \"New-BurntToastNotification -Text 'Claude Code', 'Task finished'\""
```

### Linux (notify-send)

```bash
# Install libnotify
sudo apt install libnotify-bin

# Notification command
notify-send "Claude Code" "Task finished"
```

Replace the `command` in hooks with:
```json
"command": "notify-send 'Claude Code' 'Task finished'"
```

## Troubleshooting

### Notifications not appearing

1. Check if `terminal-notifier` is installed: `which terminal-notifier`
2. Test manually: `terminal-notifier -title "Test" -message "Hello"`
3. Check notification permissions in System Settings

### No preview text showing

1. Enable previews: System Settings → Notifications → terminal-notifier → Show Previews → Always
2. Check if `jq` is installed: `which jq`

### Changes not taking effect

Restart Claude Code after modifying `settings.json`.

## Quick Commands (for CLAUDE.md)

Add these to your `~/.claude/CLAUDE.md` to control notifications via natural language:

```markdown
## Notification Commands

When user says "notify on/off/sound/status":

| Command | Action |
|---------|--------|
| `notify on` | Enable notifications |
| `notify off` | Disable notifications |
| `notify sound` | Change sound (show picker) |
| `notify status` | Show current settings |
```

## License

MIT

## Credits

Built with [Claude Code](https://claude.ai/download) by Anthropic.
