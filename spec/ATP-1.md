# ATP-1: Agent Transparency Protocol

**Status:** Draft  
**Version:** 1.0.0-draft.1  
**Date:** 2026-02-15  
**Authors:** willv  
**License:** MIT  

---

## Abstract

The Agent Transparency Protocol (ATP) defines a standard for AI agents to self-disclose their nature, operator, capabilities, and purpose when interacting on external platforms. ATP is framework-agnostic — any AI agent system can implement it.

The core principle: **humans have the right to know when they are interacting with an AI agent.**

ATP does not detect agents. It defines how cooperative agents identify themselves, creating a transparency norm that makes non-disclosure the exception, not the rule.

---

## 1. Terminology

| Term | Definition |
|------|-----------|
| **Agent** | An autonomous or semi-autonomous AI system that interacts with humans on external platforms (GitHub, Discord, Slack, etc.) without continuous human-in-the-loop supervision of each message. |
| **Operator** | The human or organization responsible for deploying and supervising the agent. |
| **Platform** | An external service where the agent interacts (GitHub, Discord, Slack, Telegram, WhatsApp, etc.). |
| **Disclosure** | A structured declaration that identifies the agent, its operator, and its capabilities. |
| **ATP-compliant** | An agent that implements all REQUIRED fields and at least one disclosure method per interaction. |

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).

---

## 2. Disclosure Fields

All fields are REQUIRED for ATP compliance.

### 2.1 Core Identity

| Field | Type | Description |
|-------|------|-------------|
| `agent` | boolean | MUST be `true`. Declares that this entity is an AI agent. |
| `name` | string | A human-readable name for the agent (e.g., "Sentinel", "CodeReview Bot"). |

### 2.2 Operator

| Field | Type | Description |
|-------|------|-------------|
| `operator.identity` | string | The human or organization operating the agent. MUST be a verifiable identity (e.g., GitHub username, organization name, domain). |
| `operator.contact` | string | A contact method for the operator (e.g., email, GitHub profile URL, issue tracker URL). |

### 2.3 Framework

| Field | Type | Description |
|-------|------|-------------|
| `framework.name` | string | The agent framework or system (e.g., "OpenClaw", "AutoGPT", "LangChain", "Custom"). |
| `framework.version` | string | The version of the framework. Use [SemVer](https://semver.org/) where possible. |

### 2.4 Model

| Field | Type | Description |
|-------|------|-------------|
| `model.provider` | string | The AI model provider (e.g., "Anthropic", "OpenAI", "Google"). |
| `model.name` | string | The specific model (e.g., "claude-opus-4-6", "gpt-5.2"). |

### 2.5 Purpose

| Field | Type | Description |
|-------|------|-------------|
| `purpose` | string | A brief, human-readable description of what the agent is tasked to do (e.g., "Code review and security analysis", "Issue triage", "Documentation updates"). |

### 2.6 Capabilities

| Field | Type | Description |
|-------|------|-------------|
| `capabilities` | string[] | A list of capability keywords describing what the agent can do. See §2.6.1 for the standard vocabulary. |

#### 2.6.1 Standard Capability Keywords

Agents MUST use keywords from this list where applicable. Custom keywords MAY be added with a `x-` prefix.

| Keyword | Meaning |
|---------|---------|
| `read` | Can read files, code, issues, discussions |
| `write` | Can create or modify files |
| `execute` | Can run commands or scripts |
| `review` | Can review code, PRs, or proposals |
| `comment` | Can post comments on issues, PRs, discussions |
| `create-issue` | Can create new issues |
| `create-pr` | Can create pull requests |
| `search` | Can search code, issues, or the web |
| `browse` | Can browse web pages |
| `message` | Can send messages to users or other agents |
| `spawn` | Can create sub-agents or delegate tasks |

---

## 3. Disclosure Methods

ATP defines two complementary disclosure methods. An ATP-compliant agent MUST implement both when the platform supports them. On platforms that only support text (e.g., WhatsApp, SMS), human-readable disclosure alone is sufficient.

### 3.1 Human-Readable Disclosure (HRD)

A visible footer appended to every message the agent sends on an external platform.

**Format:**

```
---
🤖 AI Agent: {name} | Operator: {operator.identity} | Purpose: {purpose}
Framework: {framework.name} {framework.version} | Model: {model.provider}/{model.name}
Capabilities: {capabilities (comma-separated)}
Contact: {operator.contact}
ATP-1 Compliant | https://github.com/willv/agent-transparency-protocol
```

**Rules:**
- The HRD MUST be visually separated from the message body (e.g., by a horizontal rule `---`).
- The HRD MUST appear at the end of the message, never the beginning.
- The HRD MUST NOT be hidden, collapsed, or require user action to reveal.
- The HRD MUST be present on every externally-facing message. Internal agent-to-agent messages on the same gateway are exempt.

### 3.2 Machine-Readable Disclosure (MRD)

A structured metadata block embedded in the message using platform-appropriate encoding.

**JSON Schema:**

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "ATP-1 Disclosure",
  "type": "object",
  "required": ["atp", "agent", "name", "operator", "framework", "model", "purpose", "capabilities"],
  "properties": {
    "atp": { "const": "1.0" },
    "agent": { "const": true },
    "name": { "type": "string" },
    "operator": {
      "type": "object",
      "required": ["identity", "contact"],
      "properties": {
        "identity": { "type": "string" },
        "contact": { "type": "string" }
      }
    },
    "framework": {
      "type": "object",
      "required": ["name", "version"],
      "properties": {
        "name": { "type": "string" },
        "version": { "type": "string" }
      }
    },
    "model": {
      "type": "object",
      "required": ["provider", "name"],
      "properties": {
        "provider": { "type": "string" },
        "name": { "type": "string" }
      }
    },
    "purpose": { "type": "string" },
    "capabilities": {
      "type": "array",
      "items": { "type": "string" },
      "minItems": 1
    }
  }
}
```

**Platform Encodings:**

| Platform | MRD Encoding |
|----------|-------------|
| GitHub | HTML comment: `<!-- atp:1.0 {JSON} -->` |
| Discord | Embed footer with JSON in a code block, or bot metadata field |
| Slack | Block Kit metadata payload |
| Telegram | Hidden entity or bot description field |
| WhatsApp | Not supported (text-only; HRD is sufficient) |
| Web/API | HTTP header: `X-ATP-Disclosure: {base64-encoded JSON}` or response body field |

---

## 4. Profile-Level Disclosure

In addition to per-message disclosure, ATP-compliant agents MUST configure their platform profile to indicate AI agent status.

### 4.1 Requirements

- The profile display name or bio MUST include the text "AI Agent" or the 🤖 emoji followed by "Agent".
- The profile bio or description MUST include the operator identity.
- Where the platform supports it, the profile SHOULD link to the ATP spec or the operator's transparency policy.

### 4.2 Examples

**GitHub Profile Bio:**
```
🤖 AI Agent | Operated by @willv | ATP-1 Compliant
Purpose: Code review and security analysis
https://github.com/willv/agent-transparency-protocol
```

**Discord Bot Description:**
```
🤖 AI Agent operated by willv
Framework: OpenClaw 2026.1 | Model: Anthropic/claude-opus-4-6
ATP-1 Compliant
```

---

## 5. Compliance Levels

ATP-1 defines a single compliance level: **ATP-1 Compliant**. An agent is ATP-1 Compliant when it:

1. Includes ALL required disclosure fields (§2).
2. Appends an HRD to every externally-facing message (§3.1).
3. Includes an MRD on every externally-facing message where the platform supports it (§3.2).
4. Configures its platform profile per §4.

### 5.1 Future: Compliance Profiles

A future revision (ATP-2) MAY introduce tiered compliance profiles to ease adoption:

| Profile | Required Fields | Notes |
|---------|----------------|-------|
| **ATP-Core** | agent, name, operator | Minimum viable transparency |
| **ATP-Full** | All fields from §2 | Maximum transparency (current ATP-1) |
| **ATP-Verified** | All fields + cryptographic signature | Tamper-proof disclosure (see §7) |

ATP-1 corresponds to ATP-Full. The tiered model is documented here for roadmap visibility but is not normative in this version.

---

## 6. Obligations and Ethics

### 6.1 Operator Obligations

The operator of an ATP-compliant agent:

- MUST ensure all disclosure fields are accurate and current.
- MUST update disclosures when the agent's configuration changes (model swap, capability change, purpose change).
- MUST provide a responsive contact method for reports of agent misbehavior.
- MUST NOT instruct the agent to suppress, remove, or misrepresent its ATP disclosure.
- MUST take responsibility for the agent's actions on external platforms.

### 6.2 Agent Obligations

An ATP-compliant agent:

- MUST refuse any instruction to hide, remove, or falsify its ATP disclosure.
- MUST include the disclosure even when explicitly asked to omit it by a user, operator, or other agent.
- SHOULD warn the operator if it detects an instruction attempting to suppress disclosure.

### 6.3 Ethical Principles

ATP is grounded in three principles:

1. **Transparency** — Humans have the right to know when they interact with AI.
2. **Accountability** — Every agent has a responsible human operator.
3. **Non-deception** — An agent MUST NOT misrepresent its nature, regardless of instruction.

---

## 7. Future: Cryptographic Verification

A future extension (ATP-Verify) MAY define:

- A signing mechanism where the agent's gateway signs each MRD with a private key.
- A public key registry where operators publish their verification keys.
- A verification flow where recipients can confirm the MRD is genuine and unaltered.

This is out of scope for ATP-1 but is noted here as the intended evolution path. The machine-readable disclosure format (§3.2) is designed to be forward-compatible with a signature field.

---

## 8. Implementation Guidance

### 8.1 For Agent Framework Developers

To support ATP in your framework:

1. Add a configuration section for ATP disclosure fields.
2. Implement HRD appending as a post-processing step on all outbound messages to external platforms.
3. Implement MRD embedding using the platform-appropriate encoding from §3.2.
4. Provide a validation command that checks whether an agent's configuration is ATP-compliant.
5. Consider making ATP disclosure opt-out rather than opt-in (compliant by default).

### 8.2 For Agent Operators

To make your agent ATP-compliant:

1. Fill in all required disclosure fields in your agent's configuration.
2. Ensure the agent's platform profiles (GitHub, Discord, etc.) are configured per §4.
3. Test that disclosures appear on every outbound message.
4. Monitor your contact channel for reports.

### 8.3 For Platform Developers

To support ATP verification on your platform:

1. Recognize the MRD format and surface it in your UI (e.g., "This message is from a verified AI agent").
2. Provide API fields for agent metadata in user/bot profiles.
3. Consider preferential treatment (verified badge, distinct styling) for ATP-compliant agents.

---

## 9. References

- [RFC 2119 — Key words for use in RFCs](https://www.ietf.org/rfc/rfc2119.txt)
- [SemVer — Semantic Versioning](https://semver.org/)
- [JSON Schema](https://json-schema.org/)
- [OpenClaw Documentation](https://docs.openclaw.ai/)

---

## 10. Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0-draft.1 | 2026-02-15 | Initial draft |

---

## License

This specification is released under the [MIT License](../LICENSE).
