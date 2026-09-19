# 02 — Entities & Business Rules

This document specifies the core entities, relationships, and business invariants
enforced within the Automotive Workshop domain layer.

## Core Entities

1. **Customer:** `id`, `name`, `phone`, `email`, `created_at`.
2. **Vehicle:** `id`, `customer_id`, `plate` (unique), `vin` (unique), `brand`, `model`, `year`.
3. **ServiceOrder:** `id`, `vehicle_id`, `technician_id`, `status`, `reported_failure`, `entry_at`, `exit_at`.
4. **Diagnosis:** `id`, `order_id`, `technician_id`, `findings`, `components_to_repair`, `created_at`.
5. **Intervention:** `id`, `order_id`, `description`, `labor_hours`, `parts_used`, `created_at`.
6. **Warranty:** `id`, `intervention_id`, `type` (`LABOR`|`PART`), `months`, `expires_at`.

## Inviolable Domain Invariants

- **INV-01 (Plate & VIN Uniqueness):** A vehicle must have globally unique plate and VIN.
- **INV-02 (Mechanic Single Workload):** A technician cannot hold more than one active
  service order (`IN_DIAGNOSIS` or `IN_REPAIR`) concurrently.
- **INV-03 (Strict State Transition):** Service orders transition strictly through:
  $$\text{RECEIVED} \to \text{IN\_DIAGNOSIS} \to \text{IN\_REPAIR} \to \text{READY} \to \text{DELIVERED}$$
- **INV-04 (Immutability Post-Delivery):** An order in `DELIVERED` status cannot accept
  new diagnoses, interventions, status regressions, or field updates.
- **INV-05 (Diagnosis Prerequisite):** An order cannot move to `IN_REPAIR` without an
  associated technical diagnosis record.
- **INV-06 (Warranty Expiration Determinism):** Warranty expiration is calculated as:
  $$\text{expires\_at} = \text{created\_at} + \text{months}$$

---

**Related:** [`domain-map.md`](./domain-map.md) · [`05-architecture/boundary-enforcement.md`](../05-architecture/boundary-enforcement.md)\n