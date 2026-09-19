# 04 — Functional User Stories

This document captures the functional capabilities of the Automotive Workshop MVP.

## Story Catalog

### HU-01: Vehicle & Customer Check-In
- **As a** Workshop Manager,
- **I want to** register a customer and their vehicle with plate and VIN,
- **So that** I establish an initial work order for a reported failure.
- **Acceptance Criteria:**
  - Given valid customer and vehicle data with unique plate and VIN,
  - When the manager submits the check-in form,
  - Then a new vehicle record and an order in `RECEIVED` status are created.

### HU-02: Mechanic Dispatch & Workload Invariant
- **As a** Workshop Manager,
- **I want to** assign an available technician to an open order,
- **So that** diagnostic and repair tasks begin without overloading mechanics.
- **Acceptance Criteria:**
  - Given a technician who currently has zero active orders (`IN_DIAGNOSIS` or `IN_REPAIR`),
  - When the manager assigns them to an order,
  - Then assignment succeeds and order transitions to `IN_DIAGNOSIS`.
  - Given a technician already holding an active order,
  - When assignment is attempted,
  - Then the backend rejects with HTTP 409 `MECHANIC_ALREADY_ASSIGNED`.

### HU-03: Technical Diagnosis & Repair Progression
- **As a** Technician,
- **I want to** record diagnostic findings and components to repair,
- **So that** the vehicle moves into the `IN_REPAIR` phase.
- **Acceptance Criteria:**
  - Given an assigned order in `IN_DIAGNOSIS`,
  - When the technician submits findings and components,
  - Then a diagnosis record is stored and order status advances to `IN_REPAIR`.

### HU-04: Interventions & Spare Parts Logging
- **As a** Technician,
- **I want to** log physical labor hours and installed spare parts,
- **So that** all repair work is tracked for billing and warranty coverage.
- **Acceptance Criteria:**
  - Given an order in `IN_REPAIR`,
  - When the technician submits intervention details and parts,
  - Then interventions are persisted and linked to the service order.

### HU-05: Order Completion & Delivery
- **As a** Workshop Manager,
- **I want to** mark an order as `READY` and subsequently `DELIVERED`,
- **So that** the vehicle is handed over to the customer and locked.
- **Acceptance Criteria:**
  - Given an order in `READY` status,
  - When delivery is confirmed,
  - Then status becomes `DELIVERED` and future modifications are permanently rejected.

---

**Related:** [`traceability-matrix.md`](./traceability-matrix.md) · [`non-functional.md`](./non-functional.md)\n