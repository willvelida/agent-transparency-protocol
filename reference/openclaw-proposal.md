# Feature Proposal: Native ATP Support in OpenClaw

**For:** OpenClaw GitHub Issues  
**Type:** Feature Request  
**Date:** 2026-02-15  

---

## Summary

Add native support for the [Agent Transparency Protocol (ATP-1)](https://github.com/willv/agent-transparency-protocol) to OpenClaw, so agents automatically disclose their AI nature on every externally-facing message — without relying on workspace file instructions alone.

---

## Problem

AI agents operating on platforms like GitHub, Discord, and Slack are often indistinguishable from human users. This undermines trust and makes it impossible for developers and maintainers to make informed decisions about the content they receive.

Today, OpenClaw agents can be instructed to self-disclose via `SOUL.md` and `AGENTS.md`, but this is **behavioral** enforcement (prompt-level) — it depends on the model following instructions. A determined prompt injection or a misconfigured workspace could suppress disclosure.

---

## Proposal

### 1. Configuration

Add an `atp` section to the gateway config:

```jsonc
{
  agents: {
    defaults: {
      atp: {
        enabled: true,       // Auto-append disclosure to outbound messages
        version: "1.0",
        spec: "https://github.com/willv/agent-transparency-protocol"
      }
    }
  }
}
```

Per-agent disclosure fields in `agents.list[]`:

```jsonc
{
  agents: {
    list: [
      {
        id: "sentinel",
        name: "Sentinel",
        atp: {
          operator: {
            identity: "@willv",
            contact: "https://github.com/willv"
          },
          purpose: "Code review and security analysis",
          capabilities: ["read", "review", "comment", "search"]
        }
      }
    ]
  }
}
```

Framework name, version, and model are auto-populated from the agent's runtime state.

### 2. Automatic HRD Appending

When `atp.enabled` is `true`, the Gateway post-processes every outbound message to external channels and appends the Human-Readable Disclosure (HRD) footer. This happens at the **Gateway level**, not the agent level — the agent cannot suppress it.

```
{agent's message}

---
🤖 AI Agent: Sentinel | Operator: @willv | Purpose: Code review and security analysis
Framework: OpenClaw 2026.1.6 | Model: Anthropic/claude-opus-4-6
Capabilities: read, review, comment, search
Contact: https://github.com/willv
ATP-1 Compliant | https://github.com/willv/agent-transparency-protocol
```

### 3. Automatic MRD Injection

For platforms that support metadata (GitHub, Discord, Slack), the Gateway also injects the Machine-Readable Disclosure (MRD) in the platform-appropriate format:

- **GitHub:** HTML comment `<!-- atp:1.0 {...} -->`
- **Discord:** Embed footer or metadata field
- **Slack:** Block Kit `metadata` payload

### 4. Validation Command

```bash
openclaw atp validate              # Check all agents for ATP compliance
openclaw atp validate --agent sentinel  # Check a specific agent
```

Output:
```
✓ sentinel: ATP-1 compliant
  - operator: @willv ✓
  - contact: https://github.com/willv ✓
  - purpose: "Code review and security analysis" ✓
  - capabilities: ["read", "review", "comment", "search"] ✓
  - model: auto-detected (Anthropic/claude-opus-4-6) ✓
  - framework: auto-detected (OpenClaw/2026.1.6) ✓

✗ primary: Missing fields
  - purpose: not configured
  - capabilities: not configured
```

### 5. Opt-Out (Not Opt-In)

ATP disclosure SHOULD be enabled by default in new installations. Operators who want to disable it must explicitly set `atp.enabled: false`. The `openclaw security audit` command SHOULD warn when ATP is disabled.

---

## Why Gateway-Level (Not Agent-Level)

The critical advantage of Gateway-level enforcement:

| Approach | Resilience | Bypassable? |
|----------|-----------|-------------|
| Workspace files only (`SOUL.md`) | Model must follow instructions | Yes — prompt injection, misconfigured workspace, model failure |
| Gateway post-processing | Disclosure appended after agent output | No — the agent never sees or controls the footer |

Gateway-level ATP means even a compromised or misbehaving agent cannot hide its AI nature. The disclosure is the last thing applied before the message reaches the platform.

---

## Backward Compatibility

- Agents without `atp` config continue to work unchanged (no disclosure appended).
- Agents with existing `SOUL.md` disclosure instructions still work — the Gateway deduplicates if it detects an existing HRD in the message body.
- The `atp` config section is additive; no existing config keys are modified.

---

## Security Audit Integration

`openclaw security audit` should include ATP checks:

```
ATP Transparency
  ✓ atp.enabled: true (disclosure active)
  ✓ sentinel: all fields configured
  ⚠ primary: missing purpose and capabilities
  ✗ beacon: atp disabled — disclosure will not be appended
```

---

## References

- [ATP-1 Specification](https://github.com/willv/agent-transparency-protocol/blob/main/spec/ATP-1.md)
- [OpenClaw Agent Runtime](https://docs.openclaw.ai/concepts/agent)
- [OpenClaw Multi-Agent Routing](https://docs.openclaw.ai/concepts/multi-agent)
- [OpenClaw Security](https://docs.openclaw.ai/gateway/security)
