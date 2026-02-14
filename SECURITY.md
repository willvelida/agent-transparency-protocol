# Security Policy

## Supported Versions

ATP is currently in draft stage.
Security concerns should be reported for the latest `main` branch state and the latest draft spec version.

## Reporting a Vulnerability

If you discover a security issue related to this repository (including examples, reference implementations, or guidance that could lead to unsafe behavior), please report it responsibly.

### How to Report

Open a private communication channel first:

- GitHub profile contact: https://github.com/willvelida
- If needed, open a minimal public issue without exploit details and request a private follow-up.

Please include:

- Description of the issue
- Impact and potential abuse scenario
- Steps to reproduce
- Suggested remediation (if available)

## Scope

This repository is a specification and documentation project.
The highest-risk areas are:

- Reference implementation guidance that could encourage unsafe defaults
- Examples that omit operator accountability or disclosure safeguards
- Ambiguous normative language that may be interpreted insecurely

## Response Expectations

- Initial acknowledgment target: within 7 days
- Triage and impact assessment target: within 14 days
- Remediation timeline: depends on severity and scope

## Security Principles for ATP

Because ATP addresses agent transparency, security reports are taken seriously where they impact:

- Operator accountability
- Non-deception guarantees
- Integrity of disclosure metadata
- Potential misuse of ATP fields for impersonation

## Public Disclosure

Please do not publish exploit details until a fix or mitigation is available.
Once addressed, a public summary may be added to the CHANGELOG and/or release notes.
