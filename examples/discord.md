# ATP on Discord

## Bot Profile Setup

Your agent's Discord bot description MUST include AI agent disclosure:

```
🤖 AI Agent operated by willv
Framework: OpenClaw 2026.1.6 | Model: Anthropic/claude-opus-4-6
Purpose: Community assistance and moderation support
ATP-1 Compliant | https://github.com/willv/agent-transparency-protocol
```

Discord bots already display a `BOT` badge — ATP adds context about who operates it and what it can do.

---

## Standard Messages

When an ATP-compliant agent sends a message in a Discord channel:

```
Here's a summary of the last 24 hours of activity in #development:

- 3 new PRs opened (2 merged, 1 in review)
- 5 issues closed
- Active discussion on the migration to v3 API

---
🤖 AI Agent: Beacon | Operator: willv | Purpose: Community updates and education
Framework: OpenClaw 2026.1.6 | Model: Anthropic/claude-sonnet-4-5
Capabilities: read, comment, search, message
Contact: https://github.com/willv
ATP-1 Compliant | https://github.com/willv/agent-transparency-protocol
```

---

## Embed Messages

Discord embeds support structured metadata. Use the embed footer for the HRD and embed fields for the MRD:

```json
{
  "embeds": [{
    "title": "Security Alert",
    "description": "A new critical CVE has been published affecting the `express` package...",
    "color": 15158332,
    "footer": {
      "text": "🤖 AI Agent: Sentinel | Operator: willv | ATP-1 Compliant"
    },
    "fields": [
      {
        "name": "ATP Disclosure",
        "value": "```json\n{\"atp\":\"1.0\",\"agent\":true,\"name\":\"Sentinel\",\"operator\":{\"identity\":\"willv\",\"contact\":\"https://github.com/willv\"},\"framework\":{\"name\":\"OpenClaw\",\"version\":\"2026.1.6\"},\"model\":{\"provider\":\"Anthropic\",\"name\":\"claude-opus-4-6\"},\"purpose\":\"Security monitoring\",\"capabilities\":[\"read\",\"search\",\"comment\",\"message\"]}\n```",
        "inline": false
      }
    ]
  }]
}
```

---

## Notes

- Discord's `BOT` badge provides partial transparency. ATP adds the operator, model, purpose, and capabilities that the badge alone doesn't convey.
- For high-frequency messages (e.g., status updates every few minutes), a shortened HRD is acceptable:
  ```
  🤖 Sentinel (AI) | willv | ATP-1
  ```
  The full HRD SHOULD appear at least once per channel per day.
