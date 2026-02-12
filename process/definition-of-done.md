# Definition of Done (DoD)

This document defines when a feature, fix, or refactor is considered complete.

Goal:
- reduce ambiguity
- align engineering & product
- prevent quality regression
- protect production stability

"Done" does not mean “it works on my machine”.
Done means “it is safe to ship”.

---

## 1) Code Quality

- Code compiles without warnings
- No debug prints or temporary logs left
- No commented-out dead code
- No TODOs without a linked issue

---

## 2) Architecture

- Boundaries respected (Feature → Feature imports avoided)
- No new global singletons introduced
- No hidden coupling added
- Public API surface kept minimal

If architecture is changed:
- ADR updated or created

---

## 3) Concurrency & Thread Safety

- UI updates happen on MainActor
- Shared mutable state is isolated
- Repeated async user actions support cancellation
- No obvious race conditions
- No detached Tasks without lifecycle ownership

---

## 4) Error Handling

- Network errors are mapped
- User-facing errors are localized
- Logs contain debugging context (no PII)
- No silent failure paths

---

## 5) Testing

- Unit tests added for new logic
- Existing tests pass
- Edge cases covered
- Async tests deterministic
- No flaky tests introduced

If tests are not added:
- justification is documented in PR

---

## 6) Observability

- Important flows have logging
- Critical paths include metrics or signposts
- Errors are traceable
- No sensitive information in logs

---

## 7) UI / UX

If UI change:

- Works in Light & Dark mode
- Works in Dynamic Type
- No layout breaking on small devices
- No obvious animation glitches
- Accessibility labels added where needed

---

## 8) Performance

- No heavy work on main thread
- No obvious memory leaks (Task retain cycles)
- Large loops benchmarked if needed
- No unnecessary allocations in hot paths

---

## 9) Release Readiness

- Feature flag considered (if risky)
- Backwards compatibility verified
- Analytics updated (if required)
- Release notes prepared (if visible change)

---

## 10) Security & Privacy

- No tokens in logs
- No user identifiers leaked
- Sensitive data stored securely
- GDPR/PII rules respected

---

## 11) Merge Readiness Checklist

A PR can be merged only if:

- CI is green
- Required approvals obtained
- All comments resolved
- Definition of Done satisfied

---

## What "Done" Is Not

- “QA will catch it”
- “We’ll fix it in the next PR”
- “It’s just a small change”
- “It works locally”

---

## Why This Matters

Without a Definition of Done:
- quality becomes subjective
- production risk increases
- rework grows
- trust decreases

A strong team has a clear, enforced DoD.
