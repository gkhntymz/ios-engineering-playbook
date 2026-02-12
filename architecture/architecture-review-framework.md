# Architecture Review Framework

This document defines how we review architectural proposals.

---

# 1. When is a Review Required?

Required for:

- New module introduction
- Dependency changes
- Concurrency model shift
- Storage layer change
- API contract redesign

---

# 2. Required Inputs

Every proposal must include:

- Problem statement
- Constraints
- Alternatives considered
- Trade-offs
- Migration strategy
- Rollback plan

---

# 3. Evaluation Criteria

## Scalability
Will this work with 5x traffic?

## Observability
Can we measure it?

## Failure Modes
How does it break?

## Ownership
Who maintains it?

---

# 4. Concurrency Review Checklist

- Actor isolation clear?
- Cancellation propagation?
- Thread safety validated?
- Deadlock risk analyzed?

---

# 5. Performance Review Checklist

- Benchmarks included?
- Instruments evidence?
- Memory profile checked?
- Cold start impact measured?

---

# 6. Governance Outcome

Review can result in:

- Approved
- Approved with changes
- Rejected
- Requires ADR

---

# 7. Documentation Requirement

All architectural decisions:
→ Must produce ADR.

Architecture without documentation
does not scale.
