# Slack Webhook Integration

Send terminal notifications directly to a Slack channel. Great for team visibility on long-running tasks.

## Setup

### 1. Create a Slack Webhook

1. Go to [api.slack.com/apps](https://api.slack.com/apps)
2. Click "Create New App" → "From scratch"
3. Name it "Terminal Notifications", select your workspace
4. Go to "Incoming Webhooks" → Enable
5. Click "Add New Webhook to Workspace"
6. Select a channel → Copy the webhook URL

### 2. Create the Script

```bash
#!/bin/bash
# notify-slack.sh - Send notifications to Slack

MESSAGE="${1:-Terminal notification}"
EMOJI="${2:-:computer:}"
WEBHOOK_URL="https://hooks.slack.com/services/YOUR/WEBHOOK/URL"

# Get git repo context
REPO=""
if git rev-parse --is-inside-work-tree &>/dev/null 2>&1; then
    REPO=$(basename "$(git rev-parse --show-toplevel)")
fi

# Build the message
if [ -n "$REPO" ]; then
    TEXT="$EMOJI *$REPO*: $MESSAGE"
else
    TEXT="$EMOJI $MESSAGE"
fi

curl -s -X POST \
  -H 'Content-type: application/json' \
  --data "{\"text\":\"$TEXT\"}" \
  "$WEBHOOK_URL" > /dev/null
```

### 3. Test It

```bash
chmod +x notify-slack.sh
./notify-slack.sh "Build completed successfully" ":white_check_mark:"
```

## Claude Code Integration

```json
{
  "hooks": {
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "~/.scripts/notify-slack.sh 'Claude Code task completed' ':robot_face:'"
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
            "command": "~/.scripts/notify-slack.sh 'Claude Code needs your input' ':raising_hand:'"
          }
        ]
      }
    ]
  }
}
```

## Useful Emoji

| Emoji | Code | Use Case |
|-------|------|----------|
| ✅ | `:white_check_mark:` | Success |
| ❌ | `:x:` | Failure |
| ⚠️ | `:warning:` | Warning |
| 🤖 | `:robot_face:` | AI/automation |
| 🔔 | `:bell:` | Attention needed |
| 🚀 | `:rocket:` | Deployment |

## Rich Messages (Optional)

For fancier notifications with attachments:

```bash
curl -s -X POST \
  -H 'Content-type: application/json' \
  --data '{
    "attachments": [{
      "color": "#36a64f",
      "title": "Task Completed",
      "text": "'"$MESSAGE"'",
      "footer": "Terminal Notifications",
      "ts": '"$(date +%s)"'
    }]
  }' \
  "$WEBHOOK_URL"
```

## Rate Limits

Slack has a rate limit of ~1 message per second per webhook. If you're sending notifications for every command, consider batching or using ntfy.sh instead.
