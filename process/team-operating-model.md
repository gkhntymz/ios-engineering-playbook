# Team Operating Model

This document defines how our iOS team operates at scale.

The goal is not only to ship features —  
but to ship predictably, safely, and sustainably.

---

# 1. Team Structure

## Roles

- iOS Engineers (Junior → Principal)
- Tech Lead / Staff Engineer
- Product Manager
- Designer
- QA / Automation
- Backend Partner

Clear ownership prevents coordination chaos.

---

# 2. Ownership Model

We follow a **feature ownership model**.

Each major domain has:
- A primary owner
- A secondary backup

Example:

| Domain | Owner | Backup |
|--------|--------|--------|
| Auth | Senior iOS | Mid iOS |
| Payments | Staff iOS | Senior iOS |
| Networking | Principal iOS | Staff iOS |

Ownership means:
- Reviewing PRs in that area
- Being consulted in architecture changes
- Handling production incidents

Ownership does NOT mean gatekeeping.

---

# 3. Sprint Model

We operate on:

- 2-week sprint cadence
- Trunk-based development
- Feature flags for risky features

## Sprint Flow

Week 1:
- Feature development
- Early PRs opened
- Design validation

Week 2:
- Stabilization
- QA validation
- Metrics check
- Release candidate cut

---

# 4. PR Turnaround SLA

To avoid PR stagnation:

- < 24 hours: First review required
- < 48 hours: Merge or feedback loop
- > 72 hours: Escalation to Tech Lead

Small PRs are encouraged:
- Ideal size: < 400 lines changed

---

# 5. Decision-Making Framework

We use 3 levels of decision authority:

### Level 1 – Local Decision
Engineer decides.
Examples:
- Naming
- Minor refactors

### Level 2 – Team Discussion
Small sync needed.
Examples:
- Pattern changes
- Dependency updates

### Level 3 – ADR Required
Formal documentation required.
Examples:
- Modularization
- Concurrency model
- Networking stack change

---

# 6. RFC Process

For major changes:

1. Author writes RFC draft
2. Async comments for 48 hours
3. Review meeting if needed
4. Final decision recorded in ADR

This avoids:
- Slack-only decisions
- Memory-based architecture
- Tribal knowledge

---

# 7. Production Incident Handling

## Severity Levels

P0 – App unusable  
P1 – Critical feature broken  
P2 – Degraded experience  
P3 – Minor issue  

## Incident Flow

1. Reproduce
2. Identify scope
3. Decide rollback vs hotfix
4. Communicate to stakeholders
5. Post-mortem required for P0/P1

---

# 8. Post-Mortem Format

Post-mortems are blameless.

We document:

- What happened?
- Why did it happen?
- What signals did we miss?
- What will we improve?

Output:
- Monitoring improvement
- Process update
- Automated test addition

---

# 9. Escalation Path

If blocked:

1. Peer engineer
2. Feature owner
3. Tech Lead
4. EM / Director

Clear escalation reduces silent delays.

---

# 10. Engineering Metrics

We monitor:

- PR cycle time
- Deployment frequency
- Crash-free rate
- Production incident frequency
- Code review latency

Metrics are used for system improvement, not individual punishment.

---

# 11. Knowledge Sharing

We require:

- Bi-weekly technical session
- Quarterly architecture review
- Document-first culture

Every major decision must leave a paper trail.

---

# 12. Cultural Principles

- Async-first communication
- Blameless debugging
- Small PRs
- Feature flags for safety
- Observability before optimization

---

# Why This Matters

A strong operating model:

- Reduces coordination cost
- Improves delivery predictability
- Prevents hero culture
- Scales beyond 3 engineers

This is how iOS teams evolve from
"feature factory"
to
"engineering organization".
