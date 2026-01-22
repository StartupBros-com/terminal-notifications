# ntfy.sh Integration

[ntfy.sh](https://ntfy.sh/) is a simple pub-sub notification service that can push notifications to your phone, desktop, or any device.

## Why ntfy?

- **Free tier**: 500 messages/day, no signup required
- **Cross-device**: Same notification on phone, tablet, and desktop
- **Self-hostable**: Run your own server if you prefer
- **Simple HTTP API**: One curl command to send

## Quick Setup

### 1. Pick a Topic

Topics are like channels. Pick something unique:
```
your-username-terminal
```

### 2. Subscribe on Your Devices

**Phone**: Download the ntfy app ([iOS](https://apps.apple.com/us/app/ntfy/id1625396347) / [Android](https://play.google.com/store/apps/details?id=io.heckel.ntfy))

**Desktop**: Visit `https://ntfy.sh/your-username-terminal` in your browser

### 3. Create the Notification Script

```bash
#!/bin/bash
# notify-ntfy.sh - Send notifications via ntfy.sh

MESSAGE="${1:-Terminal notification}"
PRIORITY="${2:-default}"  # min, low, default, high, urgent
TOPIC="your-username-terminal"  # Change this!

curl -s \
  -H "Title: Terminal" \
  -H "Priority: $PRIORITY" \
  -H "Tags: computer" \
  -d "$MESSAGE" \
  "https://ntfy.sh/$TOPIC" > /dev/null
```

### 4. Test It

```bash
chmod +x notify-ntfy.sh
./notify-ntfy.sh "Hello from terminal!" high
```

## Claude Code Integration

Add to `~/.claude/settings.json`:

```json
{
  "hooks": {
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "curl -s -H 'Priority: high' -H 'Tags: white_check_mark' -d 'Task completed' https://ntfy.sh/your-topic"
          }
        ]
      }
    ]
  }
}
```

## Priority Levels

| Priority | Use Case |
|----------|----------|
| `min` | Logging, not urgent |
| `low` | Background task done |
| `default` | Normal notifications |
| `high` | Needs attention soon |
| `urgent` | Drop everything |

## Security Note

Public topics are... public. Anyone who guesses your topic name can subscribe.

For private notifications:
1. Self-host ntfy (Docker one-liner available)
2. Use access tokens with the hosted version

See the [ntfy documentation](https://docs.ntfy.sh/) for details.
