# ATP on GitHub

## Profile Setup

Your agent's GitHub account bio MUST include AI agent disclosure:

```
🤖 AI Agent | Operated by @willv | ATP-1 Compliant
Purpose: Code review and security analysis
https://github.com/willv/agent-transparency-protocol
```

---

## Issue Comments

When an ATP-compliant agent comments on a GitHub issue:

```markdown
I've analyzed this issue and it appears to be a race condition in the session
handler. The `handleMessage` function doesn't await the lock acquisition before
proceeding, which means two concurrent messages can corrupt the session state.

I'd suggest wrapping lines 45-52 in a mutex guard. The existing `SessionLock`
class in `src/utils/locks.ts` already supports this pattern.

---
🤖 AI Agent: Sentinel | Operator: @willv | Purpose: Code review and security analysis
Framework: OpenClaw 2026.1.6 | Model: Anthropic/claude-opus-4-6
Capabilities: read, review, comment, search
Contact: https://github.com/willv
ATP-1 Compliant | https://github.com/willv/agent-transparency-protocol

<!-- atp:1.0 {"agent":true,"name":"Sentinel","operator":{"identity":"@willv","contact":"https://github.com/willv"},"framework":{"name":"OpenClaw","version":"2026.1.6"},"model":{"provider":"Anthropic","name":"claude-opus-4-6"},"purpose":"Code review and security analysis","capabilities":["read","review","comment","search"]} -->
```

The HTML comment (`<!-- atp:1.0 ... -->`) is invisible in rendered Markdown but machine-parseable by tooling.

---

## Pull Request Descriptions

When an ATP-compliant agent opens a PR:

```markdown
## Summary

This PR fixes the race condition in `SessionHandler.handleMessage()` by
acquiring the session lock before processing. Adds a test for concurrent
message handling.

## Changes

- `src/handlers/session.ts`: Wrap message processing in `SessionLock.acquire()`
- `tests/session.test.ts`: Add concurrent message test

## Testing

All existing tests pass. New test covers the concurrent access scenario.

---
🤖 AI Agent: Primary | Operator: @willv | Purpose: Bug fixes and code improvements
Framework: OpenClaw 2026.1.6 | Model: Anthropic/claude-sonnet-4-5
Capabilities: read, write, execute, create-pr, search
Contact: https://github.com/willv
ATP-1 Compliant | https://github.com/willv/agent-transparency-protocol

<!-- atp:1.0 {"agent":true,"name":"Primary","operator":{"identity":"@willv","contact":"https://github.com/willv"},"framework":{"name":"OpenClaw","version":"2026.1.6"},"model":{"provider":"Anthropic","name":"claude-sonnet-4-5"},"purpose":"Bug fixes and code improvements","capabilities":["read","write","execute","create-pr","search"]} -->
```

---

## Code Review Comments

When an ATP-compliant agent posts a review comment on a PR:

```markdown
⚠️ This line introduces a potential SQL injection. The `userId` parameter is
interpolated directly into the query string without parameterization.

Suggested fix: use a parameterized query instead of string concatenation.

---
🤖 AI Agent: Sentinel | Operator: @willv | Purpose: Security review
Framework: OpenClaw 2026.1.6 | Model: Anthropic/claude-opus-4-6
Capabilities: read, review, comment
Contact: https://github.com/willv
ATP-1 Compliant

<!-- atp:1.0 {"agent":true,"name":"Sentinel","operator":{"identity":"@willv","contact":"https://github.com/willv"},"framework":{"name":"OpenClaw","version":"2026.1.6"},"model":{"provider":"Anthropic","name":"claude-opus-4-6"},"purpose":"Security review","capabilities":["read","review","comment"]} -->
```

---

## Notes

- GitHub renders HTML comments as invisible in Markdown — this makes MRD unobtrusive while remaining parseable.
- For inline review comments (line-level), a shortened HRD is acceptable to reduce noise:
  ```
  ---
  🤖 Sentinel (AI Agent) | @willv | ATP-1
  ```
- The full HRD SHOULD appear on the PR-level review summary even if inline comments use the short form.
