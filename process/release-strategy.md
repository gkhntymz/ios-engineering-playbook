# Release & Hotfix Strategy

## Why this document exists

Releases are the highest-risk operational moment of any mobile system.

A strong release strategy:
- reduces production incidents
- shortens recovery time
- protects user trust
- creates predictable delivery cycles

This document defines how we ship safely.

---

# 1. Branching Model

We use a simplified trunk-based workflow with controlled release branches.

## Main Branch

- `main` is always production-ready.
- Protected by:
  - Required PR reviews
  - Required CI checks
  - No direct pushes

## Feature Branches

Naming:
- `feat/...`
- `fix/...`
- `refactor/...`
- `chore/...`

Rules:
- Short-lived
- Must pass CI
- Require review

---

# 2. Release Flow

## Standard Release Cycle

1. Code merged into `main`
2. CI must pass
3. Create a release branch:

release/1.4.0

4. Version bump
5. Final QA validation
6. App Store submission

Only critical fixes allowed after release branch cut.

---

# 3. Versioning Strategy

We follow Semantic Versioning:

MAJOR.MINOR.PATCH

- MAJOR → Breaking change
- MINOR → Feature addition
- PATCH → Bug fix

Examples:
- 1.4.0 → New feature
- 1.4.1 → Crash fix
- 2.0.0 → Architecture rewrite

---

# 4. Hotfix Strategy

Hotfixes are production emergency patches.

## When to hotfix

- Crash affecting >5% sessions
- Data corruption risk
- Security issue
- Payment flow failure

## Hotfix flow

1. Branch from production tag:

hotfix/1.4.1

2. Implement minimal fix
3. Add regression test
4. Emergency review (2 reviewers)
5. CI must pass
6. Release to App Store
7. Merge back to main

Never patch directly in App Store build.

---

# 5. Rollback Plan

Before every release, define:

- What signals indicate failure?
- How do we disable the feature?
- Can we remote-config kill switch?
- Can we server-side disable the API?

Rollback priority:

1. Feature flag disable
2. Server-side mitigation
3. App hotfix
4. App rollback

---

# 6. Release Checklist

Before submission:

- [ ] CI passing
- [ ] No TSAN warnings
- [ ] No memory leaks
- [ ] Version bumped
- [ ] Changelog updated
- [ ] Feature flags validated
- [ ] Analytics events verified

---

# 7. Production Monitoring Signals

After release monitor:

- Crash rate
- ANR rate
- Network failure rate
- Payment failure rate
- Cold start time
- Retention (Day 1)

We monitor 24h post release.

---

# 8. Ownership

Release owner rotates weekly.

Release owner responsibilities:
- Monitor metrics
- Coordinate QA
- Communicate incidents
- Trigger rollback if needed

---

# 9. Why this matters

Unstructured releases create:
- Fear-driven shipping
- Firefighting culture
- Blame-based teams

A strong release system creates:
- Psychological safety
- Predictability
- Professional engineering culture


