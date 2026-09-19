# ADR-003: MySQL 8.4 Relational Integrity & Timeline Tracking

- **Status:** Accepted
- **Date:** 2026-09
- **Deciders:** Software Architecture Team

## Context

The workshop system needs an unalterable clinical timeline per vehicle. We considered
NoSQL document stores versus relational databases.

## Decision Drivers

1. Strong relational integrity between Customers, Vehicles, Orders, and Interventions.
2. Need for global uniqueness on vehicle license plates and VINs.
3. Complex chronological timeline queries consolidating repairs over vehicle lifetime.

## Decision Outcome

We chose **MySQL 8.4** with InnoDB storage engine, using strict foreign key constraints
and indexed timeline queries.

### Positive Consequences
- Foreign keys (`ON DELETE RESTRICT`) prevent accidental deletion of vehicles with history.
- Global unique indexes guarantee zero duplicate plates or VINs.
- Clean SQL schemas align directly with relational models.

### Negative Consequences & Mitigations
- Schema changes require careful migrations.
  - *Mitigation:* Versioned DDL migration scripts executed at container initialization.

---

**Related:** [`../modular-monolith.md`](../modular-monolith.md) · [`06-data/models.md`](../../06-data/models.md)\n