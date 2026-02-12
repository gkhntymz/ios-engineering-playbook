# Technical Debt Management Framework

## Why this document exists

Technical debt is not a failure.

Unmanaged technical debt is.

High-performing teams do not eliminate debt.
They **measure, prioritize, and strategically pay it down**.

This document defines how we handle technical debt deliberately.

---

# 1. What is Technical Debt?

Technical debt includes:

- Temporary shortcuts
- Outdated dependencies
- Missing tests
- Poorly modularized code
- Architecture violations
- Performance inefficiencies
- Inconsistent patterns

Not all debt is equal.

---

# 2. Debt Classification Model

We classify debt into 4 categories:

## A. Structural Debt
Architecture-level issues
- Tight coupling
- Massive view controllers
- Missing boundaries
- Incorrect layering

## B. Code Quality Debt
- Duplicate logic
- Large methods
- Inconsistent naming
- Low test coverage

## C. Performance Debt
- Main-thread blocking
- Memory leaks
- N+1 network calls
- Inefficient rendering

## D. Operational Debt
- Missing monitoring
- No logging
- Manual release steps
- Lack of CI enforcement

---

# 3. Debt Severity Matrix

We score debt on:

Impact × Likelihood

Impact:
- Low
- Medium
- High
- Critical

Likelihood:
- Rare
- Occasional
- Frequent
- Systemic

High × Frequent = Immediate prioritization

---

# 4. Debt Visibility Rule

Debt must never live only in someone’s head.

We track debt in:

- GitHub Issues
- Jira
- ADR notes
- Dedicated "Tech Debt" label

No undocumented debt.

---

# 5. The 20% Rule

Every sprint:

- 20% capacity reserved for debt reduction
- No sprint allowed to be 100% feature-driven

If debt grows faster than feature velocity:
Product delivery will collapse.

---

# 6. When NOT to Fix Debt

Do not fix debt when:

- Feature is experimental
- Code will be replaced soon
- Refactor risk > business impact

Engineering maturity is knowing when NOT to clean.

---

# 7. Refactoring Policy

Refactoring must:

- Preserve behavior
- Be test-backed
- Be incremental
- Avoid large “big bang” rewrites

Rewrite only when:
- Architecture is fundamentally broken
- Cost of incremental fix > rewrite cost

---

# 8. Technical Debt Review Ritual

Quarterly:
- Identify top 5 structural risks
- Review performance regressions
- Re-evaluate module boundaries
- Review test coverage gaps

This is separate from sprint work.

---

# 9. Architecture Protection Mechanisms

We reduce future debt by:

- Enforcing module boundaries
- CI checks
- Static analysis
- PR review standards
- ADR documentation

Prevention > Cleanup

---

# 10. Leadership Perspective

Staff/Principal engineers:

- Model long-term thinking
- Protect architecture
- Challenge rushed shortcuts
- Quantify risk
- Communicate trade-offs clearly

Technical debt is not emotional.
It is strategic.

---

# 11. Why This Matters

Unmanaged debt leads to:

- Slower feature velocity
- Production instability
- Developer burnout
- High onboarding cost
- Talent loss

Managed debt leads to:

- Predictable delivery
- High trust culture
- Sustainable systems
