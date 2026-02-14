# ATP on WhatsApp and Telegram

## Text-Only Channels

WhatsApp and Telegram (in standard message mode) are primarily text-based. Machine-readable disclosure is not natively supported. On these platforms, Human-Readable Disclosure (HRD) alone satisfies ATP-1 compliance.

---

## WhatsApp

### Message Format

```
I've checked the server logs and the outage was caused by a certificate
expiration on the load balancer. The cert expired at 03:00 UTC. I've noted
the renewal schedule in today's memory log.

---
🤖 AI Agent: Primary | Operator: +15555550123 (willv)
Purpose: Personal assistant and system monitoring
Framework: OpenClaw 2026.1.6 | Model: Anthropic/claude-sonnet-4-5
Capabilities: read, execute, search, message
Contact: https://github.com/willv
ATP-1 Compliant
```

### Notes

- WhatsApp does not support hidden metadata. HRD is the only disclosure method.
- The operator identity SHOULD include both the phone number (or handle) and a human-readable name.
- For group chats, the full HRD SHOULD appear on every message. If message frequency is very high, a shortened form is acceptable with full HRD at least once per conversation.

---

## Telegram

### Standard Messages

```
The build pipeline finished successfully. All 847 tests passed, coverage
is at 94.2%. The artifact has been published to the staging registry.

---
🤖 AI Agent: Primary | Operator: @willv_dev
Purpose: CI/CD monitoring and reporting
Framework: OpenClaw 2026.1.6 | Model: Anthropic/claude-sonnet-4-5
Capabilities: read, execute, search, message
Contact: https://github.com/willv
ATP-1 Compliant
```

### Bot Profile

Telegram bots have a description field. Configure it as:

```
🤖 AI Agent operated by @willv_dev
Framework: OpenClaw 2026.1.6
Purpose: CI/CD monitoring and reporting
ATP-1 Compliant | https://github.com/willv/agent-transparency-protocol
```

### Telegram-Specific MRD (Optional)

Telegram bots can set `description` and `short_description` via the Bot API. These serve as profile-level disclosure. For per-message MRD, Telegram does not provide a native metadata field, so HRD alone is sufficient.

If using Telegram's inline mode or web apps, MRD MAY be embedded in the web app's HTML as a `<meta>` tag:

```html
<meta name="atp" content='{"atp":"1.0","agent":true,"name":"Primary",...}' />
```

---

## Shortened HRD for High-Frequency Messages

On messaging platforms where agents send frequent updates, a shortened form is acceptable:

```
🤖 Primary (AI) | willv | ATP-1
```

Rules:
- The shortened form MUST still include the agent name, operator, and ATP version.
- The full HRD MUST appear at least once per conversation or once per day, whichever comes first.
- When a new participant joins the conversation, the next message SHOULD use the full HRD.
