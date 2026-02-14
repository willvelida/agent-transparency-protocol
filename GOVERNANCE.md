# Governance

This document describes how decisions are made for the Agent Transparency Protocol (ATP) project.

## Project Stage

ATP is currently in an **early-stage governance model**:

- Maintainer-led decisions with transparent rationale.
- Community input is actively sought for all spec changes.
- Governance evolves as contributor participation grows.

## Roles

### Maintainer

The maintainer is responsible for:

- Curating the project roadmap.
- Triaging issues and pull requests.
- Making final decisions when consensus is unclear.
- Ensuring the project remains aligned with ATP principles.

Current maintainer:

- @willvelida

### Contributors

Contributors are anyone who:

- Opens issues.
- Reviews or comments on proposals.
- Submits pull requests.
- Improves examples, reference implementations, or documentation.

Contributors influence decisions through discussion, review, and proposal quality.

## Decision Model

### 1) Discussion First

For non-trivial changes, discussion happens in a GitHub issue before implementation.

### 2) Rough Consensus

The maintainer seeks rough consensus from active contributors.
Rough consensus means broad agreement with no serious, unresolved objections.

### 3) Maintainer Decision

If consensus is unclear, the maintainer makes a final decision and documents rationale in the issue.

## Change Types

### Editorial Changes

Examples:

- Typos
- Wording clarifications
- Non-normative examples

Process:

- PR may be merged with a single maintainer review.

### Normative Spec Changes

Examples:

- REQUIRED fields
- Compliance criteria
- RFC 2119 language
- Protocol semantics

Process:

1. Open proposal issue.
2. Minimum 7-day public discussion period.
3. Maintainer decision with written rationale.
4. Merge PR with CHANGELOG update and spec version bump if needed.

### New Reference Implementations

Process:

- PR review by maintainer.
- Must clearly distinguish framework-specific behavior from ATP requirements.

## Versioning Policy

ATP spec uses semantic-style draft versioning:

- `1.0.0-draft.N` for draft iterations
- `1.0.0` for first stable release
- `1.x` for backward-compatible clarifications/extensions
- `2.0` for breaking normative changes

## Conflict Resolution

If contributors disagree on a normative change:

1. Summarize each position in the issue.
2. Identify impact on interoperability and adoption.
3. Prefer the option that is clearer, implementable, and least surprising.
4. Maintainer decides and documents why.

## Path to Multi-Maintainer Governance

When the project has sustained contributor activity, governance may evolve to a maintainer group.
Potential trigger signals:

- 3+ regular external contributors over a 90-day window.
- Multiple merged external PRs affecting normative sections.
- Ongoing cross-framework implementation work.

At that point, the project can transition to a lightweight steering model.

## Transparency Commitment

Because ATP is itself a transparency standard, project governance must also be transparent:

- Decisions are made in public GitHub issues and PRs.
- Rationale is documented for accepted/rejected proposals.
- Roadmap and priorities are visible in project issues.
