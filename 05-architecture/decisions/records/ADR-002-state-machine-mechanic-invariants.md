# ADR-002: Strict State Machine & Mechanic Workload Invariant

- **Status:** Accepted
- **Date:** 2026-09
- **Deciders:** Software Architecture Team

## Context

The initial workshop audit revealed vulnerabilities where mechanics could be assigned
multiple active vehicles simultaneously, and delivered orders could be illegally updated.

## Decision Drivers

1. Workshop mechanics must focus on one active repair job to ensure craftsmanship.
2. Order lifecycle must be auditable and strictly linear.
3. Invariants must be enforced on the backend, not left to frontend UI validation.

## Decision Outcome

We enforce the **single active order invariant** and **linear state progression**
in the core domain use cases wrapped in MySQL serializable transactions.

```
RECEIVED -> IN_DIAGNOSIS -> IN_REPAIR -> READY -> DELIVERED (Locked)
```

### Positive Consequences
- Zero race conditions: database queries check mechanic workload atomically.
- Delivered orders become permanently immutable, preventing fraudulent updates.

### Negative Consequences & Mitigations
- Technicians cannot juggle tasks.
  - *Mitigation:* Intended business behavior confirmed with workshop management.

---

**Related:** [`ADR-003-mysql-relational-integrity.md`](./ADR-003-mysql-relational-integrity.md)\n