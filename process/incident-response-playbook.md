# Incident Response Playbook

This document defines how we respond to production incidents.

Goal:
Fast detection.
Calm response.
Clear ownership.
Documented learning.

---

# Severity Levels

P0 – App unusable / crash loop
P1 – Major feature broken
P2 – Degraded performance
P3 – Minor issue

---

# Response Timeline

## P0

- Immediate Slack alert
- Incident channel created
- Incident Commander assigned
- Rollback or feature flag within 30 minutes

---

# Roles

## Incident Commander
- Owns coordination
- Makes rollback decision

## Communications Lead
- Updates stakeholders
- Prepares external message

## Technical Lead
- Root cause investigation
- Patch creation

---

# First 10 Minutes

1. Confirm issue reproducibility
2. Check crash analytics
3. Check recent releases
4. Disable feature flag (if possible)

---

# Decision Tree

If feature flagged:
→ Disable immediately

If regression:
→ Revert release

If infrastructure:
→ Escalate to backend

---

# Post-Incident

Within 48 hours:

- Root cause analysis
- Timeline reconstruction
- Prevention plan
- Written postmortem

---

# Postmortem Template

## What happened?
## Why it happened?
## Why wasn’t it caught?
## How we fix process?
## Owner & deadline?

---

# Golden Rule

Blame the system.
Never the person.
