# Architecture Boundaries & Dependency Rules

This document defines how iOS modules should be structured and how dependencies must flow.

The goal is to:
- prevent architectural drift
- reduce tight coupling
- make scaling predictable
- keep testability high

This is not about a specific pattern (MVVM, VIPER, etc.).
This is about **dependency direction and ownership clarity**.

---

# 1️⃣ Core Principle

> Dependencies must flow inward toward stable abstractions.

Never allow feature-to-feature coupling.

---

# 2️⃣ Layer Model

A scalable iOS codebase should follow this directional flow:

App → Features → Core

---

## 🔹 App Layer

Owns:
- Composition root
- Dependency injection
- Navigation (Coordinator)
- Feature wiring
- Environment configuration
- Policy decisions (logging, retry, baseURL, etc.)

Must NOT:
- Contain business logic
- Contain networking details
- Know feature internals

---

## 🔹 Feature Modules

Own:
- Use cases
- Feature-specific business logic
- ViewModels / State management
- Public contracts (protocols, outputs)

Must NOT:
- Import other features
- Perform global navigation
- Own environment configuration

Communication must happen via:
- Output events
- Delegation
- Coordinator orchestration

---

## 🔹 Core Layer

Own:
- Networking abstractions
- Logging
- Analytics contracts
- Reusable utilities

Must NOT:
- Know about features
- Know about UI
- Know about the App layer

---

# 3️⃣ Dependency Rules

## Allowed

App → Feature  
App → Core  
Feature → Core  

## Forbidden

Feature → Feature  
Core → Feature  
Core → App  

If this rule is broken:
- refactor immediately
- introduce abstraction
- document decision with an ADR

---

# 4️⃣ Composition Root

The Composition Root is where:

- concrete implementations are created
- environment is injected
- policies are defined
- modules are wired together

Example:

AppCoordinator
    injects:
        AccountService
        AnalysisService
        LoggingService

Features only see protocols, never concrete implementations.

---

# 5️⃣ Why This Matters

Without boundaries:

- features become interdependent
- onboarding new engineers slows down
- refactors become risky
- parallel development becomes difficult
- testability decreases

With boundaries:

- features are replaceable
- testing is isolated
- scaling the team becomes easier
- architectural reasoning becomes explicit

---

# 6️⃣ Trade-offs

Strict boundaries may:

- increase boilerplate
- require more protocol definitions
- slow down early prototyping

But they:

- massively reduce long-term cost
- make principal-level reasoning possible
- support multi-team collaboration

---

# 7️⃣ Enforcement Mechanisms

Architecture rules should be enforced via:

- Swift Package boundaries
- Target isolation
- Lint rules
- PR review checklist
- ADR documentation

Architecture discipline is cultural, not technical.

---

# 8️⃣ When to Break the Rule

Only break a boundary if:

- performance constraints require it
- temporary migration is needed
- documented in ADR
- approved by tech leadership

Undocumented architectural shortcuts are technical debt.

---

# 9️⃣ Scaling Perspective

Small team:
- boundaries may feel heavy

Growing team:
- boundaries prevent chaos

Large team:
- boundaries are survival

---

# Conclusion

Architecture is not about patterns.

It is about:
- clarity
- ownership
- dependency direction
- and long-term stability.
