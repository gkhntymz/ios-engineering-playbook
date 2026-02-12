# ADR-001: Why Modularization

## Status
Accepted

## Context

As the iOS codebase grows, feature count increases and team size expands.

A monolithic architecture leads to:
- long build times
- tight coupling between features
- fragile refactors
- unclear ownership
- slow onboarding
- increased regression risk

We need a structure that supports:
- independent development
- parallel team scaling
- faster builds
- stronger boundaries
- long-term maintainability

---

## Decision

We will adopt a modular architecture based on:

App → Features → Core

Each feature will be isolated in its own module (Swift Package or target).

Features:
- must not import each other
- expose minimal public surface
- communicate via contracts (protocols / outputs)

The App layer will:
- own navigation
- own dependency injection
- wire modules together

Core layer will:
- contain reusable infrastructure
- contain no feature knowledge

---

## Rationale

### 1️⃣ Ownership clarity

Each feature module has:
- clear responsibility
- explicit API surface
- independent lifecycle

This reduces cognitive load and merge conflicts.

---

### 2️⃣ Build performance

Swift Packages allow:
- incremental builds
- faster compilation cycles
- isolated test execution

Large monolithic targets degrade build times significantly.

---

### 3️⃣ Safer refactors

With strict boundaries:
- changes stay localized
- ripple effects are minimized
- unintended cross-feature impact is reduced

---

### 4️⃣ Parallel team scaling

Multiple engineers can:
- work on separate features
- ship independently
- reduce PR contention

Modularization becomes critical after 5+ engineers.

---

### 5️⃣ Test isolation

Modules can:
- run `swift test` independently
- mock dependencies cleanly
- enforce abstraction boundaries

---

## Alternatives Considered

### Single Target + Folder Structure

Pros:
- simple
- fast to prototype

Cons:
- no compile-time isolation
- easy to violate boundaries
- architectural drift over time

Rejected for long-term scalability.

---

### Micro-framework per feature

Pros:
- strong isolation

Cons:
- build complexity
- versioning overhead
- overkill for mid-size teams

Chosen approach balances simplicity and isolation.

---

## Trade-offs

Modularization introduces:
- more protocols
- more boilerplate
- increased initial setup

However:
- reduces long-term maintenance cost
- improves engineering quality
- supports growth

---

## Consequences

Positive:
- improved scalability
- better testability
- clearer ownership
- safer concurrency boundaries

Negative:
- slower early iteration
- more up-front design decisions required

---

## When to Revisit

Re-evaluate if:
- team size remains very small (<3 engineers)
- product scope remains extremely narrow
- build system complexity outweighs benefits

Otherwise, modularization is a long-term investment.

---

## Production Perspective

At scale, modularization is not an optimization.

It is a survival requirement.

Without it:
- regressions multiply
- refactors stall
- knowledge silos form

With it:
- architecture becomes explainable
- boundaries are enforceable
- scaling becomes predictable
