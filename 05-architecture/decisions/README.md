# 05 — Architectural Decision Records (ADRs)

This directory maintains the log of significant architectural and design decisions
for the Automotive Technical Support Management System.

## ADR Log

| ADR Number | Title | Status | Date |
|---|---|---|---|
| [`ADR-001`](./records/ADR-001-go-clean-architecture.md) | Clean Architecture Modular Monolith in Go 1.25 | Accepted | 2026-09 |
| [`ADR-002`](./records/ADR-002-state-machine-mechanic-invariants.md) | Strict State Machine & Mechanic Workload Invariant | Accepted | 2026-09 |
| [`ADR-003`](./records/ADR-003-mysql-relational-integrity.md) | MySQL 8.4 Relational Constraints & Timeline Integrity | Accepted | 2026-09 |

## Decision Principles

1. **Explicit Invariants:** Business constraints must be captured as formal ADRs.
2. **Trade-off Analysis:** Every ADR documents positive and negative trade-offs.
3. **No Retrospective Excuses:** Decisions are made before coding starts.

## ADR Lifecycle

1. **Proposed:** Decision documented and circulated for engineering review.
2. **Accepted:** Consensus reached; decision implemented in code.
3. **Superseded:** Replaced by a newer numbered ADR.

---

**Related:** [`_template-adr.md`](./_template-adr.md) · [`../README.md`](../README.md)\n