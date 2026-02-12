# Branching & Governance Model

This document defines how we manage source control,
merge policies, and release governance in a scalable iOS organization.

The goal is:

- Safe iteration
- Predictable releases
- Minimal merge conflicts
- Clear ownership
- Production stability

---

# 1. Branching Strategy

We follow a **Trunk-Based Development with Protected Main** approach.

## Branch Types

### main
- Always production-ready
- Protected branch
- Requires PR + review
- CI must pass
- No direct commits allowed

---

### feature/*
Short-lived branches.

Examples:
- feature/login-refactor
- feature/analytics-pipeline
- feature/payment-retry

Rules:
- Branch from main
- Rebase frequently
- Merge within 3–5 days
- Small PRs preferred

---

### hotfix/*
Used only for production incidents.

Examples:
- hotfix/crash-ios17
- hotfix/payment-timeout

Rules:
- Branch from latest release tag
- Minimal change
- Merge back to main
- Tag immediately

---

### release/*
Optional. Used for larger teams.

Example:
- release/1.4.0

Used when:
- QA stabilization needed
- Feature freeze window required

---

# 2. Why Not Git Flow?

We avoid full Git Flow because:

- Long-lived branches increase merge complexity
- Diverging histories increase risk
- Harder to reason about production state

Trunk-based reduces:
- Merge debt
- Integration risk
- Hidden regressions

---

# 3. Pull Request Governance

## PR Requirements

Every PR must:

- Link to issue/ticket
- Include description of change
- Explain risk surface
- Include screenshots (if UI change)
- Pass CI

---

## PR Approval Rules

- Minimum 1 reviewer (Senior+)
- 2 reviewers for architectural changes
- ADR required if affecting core modules

---

## PR Size Guidelines

Ideal PR:
- < 400 lines
- Single responsibility
- Testable in isolation

Large PRs increase:
- Review latency
- Risk of regression
- Reviewer fatigue

---

# 4. CI Requirements

Before merging to main:

- Build must succeed
- Unit tests must pass
- Lint checks must pass
- No new warnings
- Crash reproduction test must pass (if bug fix)

Optional:
- Snapshot tests
- Performance regression checks

---

# 5. Code Freeze Policy

For major releases:

- Code freeze 3 days before release
- Only P1/P0 fixes allowed
- No new features
- No refactors

This reduces last-minute regression risk.

---

# 6. Release Tagging

Every production release must:

- Be tagged
- Include changelog entry
- Reference release notes

Example:

git tag -a 1.4.0 -m "iOS 1.4.0 Production Release"
git push origin 1.4.0

Tags provide:
- Rollback anchor
- Reproducibility
- Auditability

---

# 7. Governance Levels

We define 3 levels of governance:

### Low Risk Changes
- UI tweaks
- Copy updates
- Minor refactors

Approval: 1 reviewer

---

### Medium Risk Changes
- Networking layer updates
- Concurrency adjustments
- Performance optimizations

Approval: 2 reviewers

---

### High Risk Changes
- Architecture refactor
- Dependency replacement
- Modularization change

Approval:
- ADR required
- Tech Lead sign-off

---

# 8. Feature Flags Policy

All high-risk features must:

- Be behind feature flag
- Have remote kill switch
- Be measurable

Feature flags enable:
- Gradual rollout
- Fast rollback
- A/B testing

---

# 9. Rollback Strategy

If production issue occurs:

Option A:
- Feature flag disable

Option B:
- Hotfix branch

Option C:
- Revert commit

Preferred order:
Flag → Revert → Hotfix

---

# 10. Conflict Resolution Policy

If long-running branch diverges:

- Rebase onto latest main
- Resolve conflicts locally
- Never merge main into feature repeatedly

Frequent rebase reduces integration shock.

---

# 11. Governance Anti-Patterns

We explicitly avoid:

- Direct pushes to main
- Unreviewed hotfixes
- Massive PRs
- Architecture changes without ADR
- Silent rollouts

---

# 12. Why This Matters

Strong branching governance:

- Reduces production incidents
- Improves review quality
- Makes scaling predictable
- Enables distributed teams

Branching strategy is not about Git.

It is about **risk management**.
