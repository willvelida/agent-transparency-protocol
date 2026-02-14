# ATP on Slack

## Bot Profile Setup

Your agent's Slack app profile MUST include AI agent disclosure:

- **Display Name:** `Sentinel 🤖`
- **Description:** `AI Agent operated by @willv | Framework: OpenClaw | ATP-1 Compliant`
- **About:** Link to `https://github.com/willv/agent-transparency-protocol`

---

## Block Kit Messages

Slack's Block Kit supports structured metadata. Use it for both HRD and MRD:

```json
{
  "blocks": [
    {
      "type": "section",
      "text": {
        "type": "mrkdwn",
        "text": "I've reviewed the deployment checklist for v2.4. Everything looks good — the staging tests passed and the rollback plan is documented. One suggestion: consider adding a healthcheck endpoint to the canary before promoting."
      }
    },
    {
      "type": "divider"
    },
    {
      "type": "context",
      "elements": [
        {
          "type": "mrkdwn",
          "text": "🤖 AI Agent: Sentinel | Operator: @willv | Purpose: Deployment review\nFramework: OpenClaw 2026.1.6 | Model: Anthropic/claude-opus-4-6\nCapabilities: read, review, comment | ATP-1 Compliant"
        }
      ]
    }
  ],
  "metadata": {
    "event_type": "atp_disclosure",
    "event_payload": {
      "atp": "1.0",
      "agent": true,
      "name": "Sentinel",
      "operator": {
        "identity": "@willv",
        "contact": "https://github.com/willv"
      },
      "framework": { "name": "OpenClaw", "version": "2026.1.6" },
      "model": { "provider": "Anthropic", "name": "claude-opus-4-6" },
      "purpose": "Deployment review",
      "capabilities": ["read", "review", "comment"]
    }
  }
}
```

---

## Notes

- Slack's `metadata` field (Block Kit) is not visible to users but is accessible via the API — ideal for MRD.
- The `context` block provides the human-readable disclosure in a visually subdued format.
- Slack apps already show an `APP` badge. ATP adds the context Slack doesn't provide natively.
