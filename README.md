# Terminal Notifications

**Never miss when a long-running command finishes.**

Cross-platform notification scripts for Mac, Windows (WSL2), and Linux—plus integration guides for Claude Code, ntfy.sh, and Slack.

📖 **Full tutorial**: [startupbros.com/claude-code-notifications](https://startupbros.com/claude-code-notifications/)

## Quick Start

### macOS

```bash
# Download
curl -o ~/.local/bin/notify https://raw.githubusercontent.com/startupbros/terminal-notifications/main/scripts/notify-mac.sh
chmod +x ~/.local/bin/notify

# Test
notify "Hello from terminal!" complete
```

### Windows (WSL2)

```bash
# Download
mkdir -p ~/.local/bin
curl -o ~/.local/bin/notify https://raw.githubusercontent.com/startupbros/terminal-notifications/main/scripts/notify-windows.sh
chmod +x ~/.local/bin/notify

# Test
notify "Hello from terminal!" complete
```

### Linux

```bash
# Install dependencies (Ubuntu/Debian)
sudo apt install libnotify-bin pulseaudio-utils

# Download
curl -o ~/.local/bin/notify https://raw.githubusercontent.com/startupbros/terminal-notifications/main/scripts/notify-linux.sh
chmod +x ~/.local/bin/notify

# Test
notify "Hello from terminal!" complete
```

## Usage

```bash
notify "Your message" [sound_type]
```

Sound types: `input`, `complete`, `default`, `none`

## Claude Code Integration

Add to `~/.claude/settings.json`:

```json
{
  "hooks": {
    "Notification": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "~/.local/bin/notify 'Awaiting your input' input"
          }
        ]
      }
    ],
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "~/.local/bin/notify 'Task completed' complete"
          }
        ]
      }
    ]
  }
}
```

## What's Inside

```
terminal-notifications/
├── scripts/
│   ├── notify-mac.sh              # macOS (AppleScript)
│   ├── notify-mac-terminal-notifier.sh  # macOS (terminal-notifier)
│   ├── notify-windows.sh          # Windows/WSL2 (PowerShell)
│   └── notify-linux.sh            # Linux (notify-send)
├── claude-code/
│   └── settings-example.json      # Claude Code hooks config
├── extras/
│   ├── ntfy-integration.md        # Push to phone
│   └── slack-webhook.md           # Team notifications
└── README.md
```

## Sound Options

| Type | Mac | Windows | Linux |
|------|-----|---------|-------|
| `input` | Funk | Windows Exclamation | dialog-warning |
| `complete` | Hero | tada.wav | complete |
| `default` | Pop | Windows Default | message |

## Going Further

- **[ntfy.sh](extras/ntfy-integration.md)**: Push notifications to your phone
- **[Slack](extras/slack-webhook.md)**: Notify your team

## License

MIT - Do whatever you want with it.

---

Made by [StartupBros](https://startupbros.com) · [Full Tutorial](https://startupbros.com/claude-code-notifications/)
