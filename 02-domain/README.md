# 02 — Business Domain

The domain section formalizes the core entities, business invariants, lifecycle rules,
and bounded contexts governing the Automotive Technical Support Management System.

## Contents

| Document | Purpose |
|---|---|
| [`domain-map.md`](./domain-map.md) | Subdomain classification and core entity interactions |
| [`entities-and-rules.md`](./entities-and-rules.md) | Entity attributes, relationships, and deterministic domain invariants |
| [`module-boundaries.md`](./module-boundaries.md) | Boundary enforcement in the Go Clean Architecture modular monolith |

## Domain Overview

At the heart of the system is the **Service Order Aggregate**, which connects a
registered **Vehicle** to a responsible **Technician** and tracks clinical events
through diagnostic, repair, and warranty stages.

```
Customer --1:N--> Vehicle --1:N--> ServiceOrder --1:1--> Diagnosis
                                       |
                                       +--1:N--> Intervention --1:1--> Warranty
```

---

**Related:** [`domain-map.md`](./domain-map.md) · [`entities-and-rules.md`](./entities-and-rules.md)\n