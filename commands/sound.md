---
description: Change notification sound
---

# Change Notification Sound

Change the sound used for notifications.

## Usage

```
/notify:sound
```

## Instructions

When this command is invoked:

1. Use AskUserQuestion to present sound options:

```
Question: "Which notification sound do you prefer?"
Header: "Sound"
Options:
- "Pop" (default) - Short, subtle pop sound
- "Glass" - Clear glass tap
- "Ping" - Soft ping
- "Funk" - Funky alert
- "Basso" - Deep bass note
```

2. After user selects, read `~/.claude/settings.json`
3. Update the `-sound` parameter in all notification hooks to use the selected sound
4. Write the updated settings.json
5. Inform the user: "Sound changed to [selected]! Restart Claude Code for changes to take effect."

## Available macOS Sounds

- Pop (default)
- Glass
- Ping
- Funk
- Basso
- Blow
- Bottle
- Frog
- Hero
- Morse
- Purr
- Sosumi
- Submarine
- Tink
