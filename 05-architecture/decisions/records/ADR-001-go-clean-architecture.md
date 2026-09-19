# ADR-001: Clean Architecture Modular Monolith in Go 1.25

- **Status:** Accepted
- **Date:** 2026-09
- **Deciders:** Software Architecture Team

## Context

The Automotive Workshop System requires high data integrity, strict state management,
and fast response times. We evaluated whether to use microservices or a modular monolith,
and selected the backend language.

## Decision Drivers

1. Need for ACID transactions across order status updates and mechanic assignments.
2. Simplified deployment on local workshop servers via Docker.
3. High throughput with minimal memory footprint.

## Decision Outcome

We decided to build a **Modular Monolith using Go 1.25 Clean Architecture**.

### Positive Consequences
- Fast compilation and native execution with minimal memory consumption (< 50MB RSS).
- Domain rules are completely decoupled from persistence and HTTP frameworks.
- Single binary simplifies containerization with Docker.

### Negative Consequences & Mitigations
- Go requires explicit error handling boilerplate.
  - *Mitigation:* Established standard error wrapping conventions.

---

**Related:** [`ADR-002-state-machine-mechanic-invariants.md`](./ADR-002-state-machine-mechanic-invariants.md)\n