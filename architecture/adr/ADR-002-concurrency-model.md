# ADR-002: Concurrency Model & Isolation Strategy

## Status
Accepted

## Context

Modern iOS apps run multiple concurrent workloads:
- network requests
- disk/cache reads
- image processing
- analytics/logging
- UI-driven async flows (search-as-you-type, pagination, refresh)

Without a clear concurrency strategy, we risk:
- data races
- stale UI updates
- runaway background work
- memory leaks via long-lived tasks
- hard-to-debug production issues

We need a default model that:
- is safe by default
- makes ownership explicit
- supports cancellation
- remains debuggable in production

---

## Decision

We adopt **Swift Concurrency (async/await)** as the primary concurrency model.

### Default rules

1) Prefer **structured concurrency**
- `Task {}` inside an owned scope
- `async let` or task groups for parallel work
- avoid detached tasks unless explicitly justified

2) Use **Actors** for shared mutable state
- token stores
- caches
- in-memory registries
- counters/state accessed from multiple tasks

3) Use **@MainActor** for UI-facing state
- ViewModel outputs that drive UIKit/SwiftUI
- navigation triggers
- UI updates

4) Cancellation is mandatory for user-driven flows
- search-as-you-type
- screen switching
- refresh / repeated queries
- ViewModel deinit must cancel tasks

5) Keep concurrency boundaries observable
- logging around task start/end/cancel
- signposts for performance-critical flows
- TSAN in CI for concurrency regression detection

---

## Rationale

### 1) Structured concurrency reduces runaway work

Tasks inherit:
- priority
- cancellation
- lifetime scope

This prevents “orphaned” background work.

---

### 2) Actors reduce race conditions by construction

Actors enforce:
- single-threaded access to internal state
- isolation at compile-time

Actors are preferred over locks/queues when:
- correctness matters more than micro-optimizations
- state is truly shared

---

### 3) Cancellation must be cooperative

Cancellation in Swift is:
- cooperative, not preemptive

Therefore long loops must:
- check `Task.isCancelled`
- call `Task.checkCancellation()` at safe points

---

### 4) UI correctness requires MainActor discipline

Without MainActor boundaries:
- UI updates can happen off-main
- random crashes or warnings occur
- stale results can overwrite newer ones

---

## Alternatives Considered

### GCD as the primary model

Pros:
- widely known
- low-level control

Cons:
- no structured lifetimes
- cancellation is manual and fragile
- weak ownership semantics
- increases production debugging cost

Rejected as default.

---

### OperationQueue

Pros:
- supports cancellation semantics
- dependencies between operations

Cons:
- verbose
- still not integrated with structured concurrency
- bridging complexity with async/await

Kept as legacy/compatibility tool only.

---

## Trade-offs

Swift Concurrency introduces:
- async boundaries everywhere
- actor overhead in some hot paths
- learning curve

But it provides:
- safer defaults
- clearer lifetimes
- production-friendly cancellation
- better reasoning under load

---

## Consequences

Positive:
- fewer data races
- explicit cancellation across flows
- consistent UI thread safety
- easier root-cause analysis

Negative:
- engineers must learn actor isolation rules
- some high-performance areas may require careful design

---

## Enforcement

We enforce this strategy through:
- PR checklist items (cancellation, @MainActor, actor usage)
- TSAN runs on selected demo targets
- docs and examples in this repository

---

## When to Revisit

Revisit if:
- measurable actor overhead becomes a bottleneck
- new Swift evolution changes actor behavior significantly
- we adopt a different architecture requiring alternative isolation primitives

---

## Production Notes

Concurrency bugs are often:
- intermittent
- device-specific
- timing-dependent

Therefore the strategy prioritizes:
- determinism
- observability
- cancellation discipline
over micro-optimizations.

Correctness first, performance second — then measure and optimize.
