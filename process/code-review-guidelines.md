# Code Review Guidelines (PR Review)

This document defines how we review code in a production iOS team.

Goal:
- ship safely
- maintain quality at scale
- spread context
- reduce long-term maintenance cost

Code review is not a gatekeeping tool.
It is a collaboration and risk management practice.

---

## 1) Principles

### 1.1 Review for risk, not style
The priority order:
1) Correctness
2) Security & privacy
3) Performance & resource usage
4) Maintainability
5) Readability and style

Style should be automated (SwiftFormat / SwiftLint).
Humans focus on risk.

---

### 1.2 The author owns the change
The reviewer does not “take responsibility” for the change.
The reviewer helps identify risks and missing considerations.

---

### 1.3 Small PRs win
Prefer:
- short-lived branches
- incremental changes
- PRs that can be reviewed in < 20 minutes

Large PRs produce shallow reviews.

---

## 2) PR Expectations (Author)

Before requesting review, ensure:

- PR title is descriptive
- description explains the intent
- screenshots are added for UI changes
- logging is safe (no PII)
- tests are added or explicitly justified
- rollout impact is considered

### PR description template
Use this structure:

- What changed:
- Why:
- How to test:
- Risk:
- Rollout / Monitoring:

---

## 3) Reviewer Checklist

### 3.1 Correctness
- Are edge cases handled?
- Are error paths correct?
- Can stale data overwrite newer results?
- Does cancellation exist for user-driven async flows?

### 3.2 Concurrency
- Is shared mutable state isolated?
- Are UI updates on `@MainActor`?
- Are tasks properly cancelled on reuse/deinit?
- Any risk of race conditions?

### 3.3 API / Architecture
- Does this break boundaries (Feature → Feature)?
- Is the public surface minimal?
- Is composition root respected?
- Any hidden coupling introduced?

### 3.4 Testing
- Is there a unit test for logic?
- Is there UI/integration coverage where needed?
- Are tests deterministic?

### 3.5 Observability
- Are failures logged with useful context?
- Are logs redacted?
- Is there a metric or breadcrumb where it matters?

### 3.6 Performance
- Any unnecessary work on the main thread?
- Any loops / allocations that scale with user data?
- Any obvious memory leaks (Task retain cycles, closures)?

---

## 4) Comment Categories (Shared Vocabulary)

To reduce ambiguity, use labels:

- **Blocker:** must be fixed before merge
- **Suggestion:** strongly recommended, but optional
- **Question:** clarify intent
- **Nit:** minor style/readability (avoid too many)
- **Praise:** highlight a good practice (sparingly)

Example:
- "Blocker: UI update must be on MainActor."
- "Suggestion: extract mapper to keep service smaller."
- "Question: what happens if request returns stale data?"
- "Nit: rename for clarity."

---

## 5) Review SLA (Speed Expectations)

- A PR should receive first feedback within 1 business day.
- If a PR is urgent, communicate explicitly (Slack + reason).
- Review speed is a team performance metric.

Slow reviews = slow delivery = bigger PRs = more risk.

---

## 6) Approval Rules

Default:
- 1 approval for small changes
- 2 approvals for risky areas (auth, payments, networking, security)

A PR must not be merged if:
- tests fail
- conversations unresolved
- architecture boundaries are violated

---

## 7) Common Red Flags

- "quick fix" that bypasses boundaries
- “temporary” hacks without issue/ADR
- global singletons
- silent error handling
- missing cancellation in repeated user actions
- UI updates off-main
- logging tokens or user identifiers
- unclear ownership of new code

---

## 8) High-quality review examples

Good:
- "This introduces a new shared mutable cache. Can we isolate it behind an Actor?"
- "Search is triggered on every keystroke. Where is cancellation handled?"
- "Error mapping: do we have a user-safe message and a debug log?"

Not good:
- "Looks fine."
- "Ship it."
- nitpicking formatting that tooling should handle

---

## 9) Culture

We optimize for:
- clarity
- kindness
- shared learning

We do not optimize for:
- ego
- perfectionism
- personal style preferences

A strong team makes reviews safe and predictable.
