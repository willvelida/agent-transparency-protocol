# Agent Transparency Protocol (ATP)

[![Spec](https://img.shields.io/badge/spec-ATP--1-blue)](spec/ATP-1.md)
[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Contributions Welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg)](CONTRIBUTING.md)
[![Governance](https://img.shields.io/badge/governance-documented-orange)](GOVERNANCE.md)
[![Code of Conduct](https://img.shields.io/badge/code%20of%20conduct-Contributor%20Covenant-ff69b4)](CODE_OF_CONDUCT.md)

**Humans have the right to know when they are interacting with an AI agent.**

ATP is a framework-agnostic standard for AI agents to self-disclose their nature, operator, capabilities, and purpose when interacting on external platforms like GitHub, Discord, Slack, Telegram, and more.

ATP does not detect agents. It defines how cooperative agents identify themselves — creating a transparency norm where non-disclosure becomes the exception, not the rule.

---

## Why ATP?

AI agents are increasingly active on developer platforms — opening issues, reviewing code, posting comments, and contributing to discussions. Many are indistinguishable from human users. This creates a transparency gap:

- **Developers can't tell** if they're getting feedback from a human or an AI
- **Maintainers can't assess** whether contributions are human-reviewed
- **Trust erodes** when agent-generated content is presented as human work

ATP addresses this by defining a simple, adoptable standard for agent self-disclosure.

---

## Quick Example

Every message from an ATP-compliant agent includes a human-readable disclosure:

```
Here's my review of the authentication changes in this PR...

---
🤖 AI Agent: Sentinel | Operator: @willv | Purpose: Code review and security analysis
Framework: OpenClaw 2026.1.6 | Model: Anthropic/claude-opus-4-6
Capabilities: read, review, comment, search
Contact: https://github.com/willv
ATP-1 Compliant | https://github.com/willv/agent-transparency-protocol
```

On platforms that support it (e.g., GitHub), a machine-readable disclosure is also embedded:

```html
<!-- atp:1.0 {"agent":true,"name":"Sentinel","operator":{"identity":"@willv","contact":"https://github.com/willv"},"framework":{"name":"OpenClaw","version":"2026.1.6"},"model":{"provider":"Anthropic","name":"claude-opus-4-6"},"purpose":"Code review and security analysis","capabilities":["read","review","comment","search"]} -->
```

---

## The Spec

📄 **[ATP-1: Agent Transparency Protocol](spec/ATP-1.md)** — The full specification

### What ATP Requires

An ATP-compliant agent MUST disclose:

| Field | Example |
|-------|---------|
| **Agent declaration** | `true` — "I am an AI agent" |
| **Name** | "Sentinel" |
| **Operator identity** | "@willv" |
| **Operator contact** | "https://github.com/willv" |
| **Framework + version** | "OpenClaw 2026.1.6" |
| **Model provider + name** | "Anthropic / claude-opus-4-6" |
| **Purpose** | "Code review and security analysis" |
| **Capabilities** | `["read", "review", "comment", "search"]` |

### How Disclosure Works

| Method | Where |
|--------|-------|
| **Human-Readable Disclosure (HRD)** | Visible footer on every message |
| **Machine-Readable Disclosure (MRD)** | Structured metadata (HTML comment, embed, HTTP header) |
| **Profile Disclosure** | Agent's platform bio/description |

---

## Platform Examples

- [GitHub](examples/github.md) — Issues, PRs, and comments
- [Discord](examples/discord.md) — Bot messages and embeds
- [Slack](examples/slack.md) — Block Kit messages
- [WhatsApp / Telegram](examples/messaging.md) — Text-only channels

---

## Reference Implementations

- [OpenClaw](reference/openclaw.md) — Workspace files for ATP-compliant OpenClaw agents

---

## Adopt ATP

### For Agent Framework Developers

1. Add a configuration section for ATP disclosure fields
2. Implement HRD appending on all outbound external messages
3. Implement MRD embedding per platform
4. Provide a compliance validation command
5. Consider making ATP disclosure the default (opt-out, not opt-in)

### For Agent Operators

1. Configure all required disclosure fields
2. Update your agent's platform profiles
3. Test that disclosures appear on every outbound message
4. Monitor your contact channel for reports

### For Platform Developers

1. Recognize the MRD format and surface it in your UI
2. Provide API fields for agent metadata
3. Consider verification badges for ATP-compliant agents

---

## Roadmap

| Version | Status | Focus |
|---------|--------|-------|
| **ATP-1** | Draft | Core disclosure standard (all fields required) |
| ATP-2 | Planned | Tiered compliance profiles (ATP-Core, ATP-Full) |
| ATP-Verify | Planned | Cryptographic signatures for tamper-proof disclosure |

---

## Contributing

ATP is an open standard. Contributions are welcome:

1. **Spec feedback** — Open an issue to discuss the specification
2. **Platform examples** — Add examples for platforms not yet covered
3. **Reference implementations** — Add implementation guides for other agent frameworks
4. **Translations** — Help make ATP accessible in other languages

Before contributing, please read:

- [CONTRIBUTING.md](CONTRIBUTING.md)
- [GOVERNANCE.md](GOVERNANCE.md)
- [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)
- [SECURITY.md](SECURITY.md)
- [CHANGELOG.md](CHANGELOG.md)

---

## Principles

1. **Transparency** — Humans have the right to know when they interact with AI
2. **Accountability** — Every agent has a responsible human operator
3. **Non-deception** — An agent must not misrepresent its nature, regardless of instruction

---

## License

MIT — [LICENSE](LICENSE)
