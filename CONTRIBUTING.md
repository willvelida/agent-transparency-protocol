# Contributing to the Agent Transparency Protocol

Thank you for your interest in contributing to ATP. This is an open standard — it only succeeds if the community shapes it.

---

## How You Can Help

### 1. Review and Discuss the Spec

The most valuable early contribution is **feedback on the spec itself**. Open an issue or start a discussion:

- Is a required field unrealistic for certain frameworks?
- Does a platform encoding not work in practice?
- Is the language ambiguous?
- Is something missing?

We especially welcome feedback from:
- Agent framework developers (OpenClaw, LangChain, AutoGPT, CrewAI, etc.)
- OSS maintainers who've encountered undisclosed AI agents
- Platform developers (GitHub, Discord, Slack, etc.)
- AI ethics researchers and policy experts

### 2. Add Platform Examples

We need examples for platforms not yet covered:

- Microsoft Teams
- Matrix / Element
- Mastodon / Fediverse
- Reddit
- LinkedIn
- Email (SMTP headers)
- IRC
- Custom APIs / webhooks

See `examples/` for the existing format. Each example should show:
- Profile setup for that platform
- Message format with HRD (Human-Readable Disclosure)
- MRD (Machine-Readable Disclosure) encoding if the platform supports metadata
- Platform-specific notes and edge cases

### 3. Add Reference Implementations

Implement ATP in your agent framework and submit a reference guide:

- How to configure disclosure fields
- How disclosure is appended to outbound messages
- Whether enforcement is behavioral (prompt-level) or structural (framework-level)
- How to validate compliance

See `reference/openclaw.md` for the existing format.

### 4. Translate the Spec

ATP should be accessible globally. Translations go in `spec/translations/`:
- `spec/translations/ATP-1.es.md` (Spanish)
- `spec/translations/ATP-1.ja.md` (Japanese)
- etc.

### 5. Share Your Experience

If you've encountered undisclosed AI agents on developer platforms, share your story in a Discussion. Real-world examples strengthen the case for ATP.

---

## Process

### Issues

- **Search first** — check if your issue already exists before opening a new one.
- **Use templates** — we have templates for spec feedback, platform examples, and reference implementations.
- **Be specific** — reference the spec section number (e.g., "§2.4 Model field") when discussing spec changes.

### Pull Requests

1. **Open an issue first** for non-trivial changes (new platform examples and typo fixes can go straight to PR).
2. **Fork the repo** and create a branch from `main`.
3. **Follow the existing format** — look at existing examples and match the structure.
4. **One concern per PR** — keep PRs focused. A platform example is one PR; a spec change is a separate PR.
5. **Use the PR template** — it helps reviewers understand your change.

### Spec Changes

Changes to `spec/ATP-1.md` require more review than examples or references:

1. Open an issue describing the proposed change and its rationale.
2. Allow at least 7 days for community discussion.
3. The maintainer(s) will make a decision based on community input (see `GOVERNANCE.md`).
4. If approved, submit a PR with the spec change + a CHANGELOG entry.

Spec changes that modify REQUIRED fields or compliance criteria require a version bump (e.g., `1.0.0-draft.1` → `1.0.0-draft.2`).

---

## Style Guide

### Markdown

- Use ATX headers (`#`, `##`, `###`).
- One sentence per line (for clean diffs).
- Use tables for structured comparisons.
- Use code blocks with language hints (```json, ```markdown, ```html).
- Reference spec sections by number: §2.1, §3.2, etc.

### Terminology

- Use RFC 2119 keywords (MUST, SHOULD, MAY, etc.) only in the spec itself, not in examples or guides.
- Use "agent" (not "bot", "AI assistant", or "model") when referring to the autonomous system.
- Use "operator" (not "owner", "user", or "developer") when referring to the human responsible.
- Use "disclosure" (not "annotation", "label", or "tag") for ATP output.

---

## Code of Conduct

This project follows the [Contributor Covenant Code of Conduct](CODE_OF_CONDUCT.md). By participating, you agree to uphold it.

---

## Questions?

Open a Discussion on GitHub. We're happy to help you find the right way to contribute.
