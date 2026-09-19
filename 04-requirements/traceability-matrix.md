# 04 — Traceability Matrix

The traceability matrix maps business requirements to domain entities, business invariants,
API endpoints, and automated verification tests.

| Story ID | Domain Entity | Invariant | API Endpoint | Verification Test |
|---|---|---|---|---|
| **HU-01** | Customer, Vehicle | INV-01 (Plate/VIN Unique) | `POST /api/v1/vehicles` | `TestVehicleCheckIn_UniquePlate` |
| **HU-02** | ServiceOrder, User | INV-02 (Single Workload) | `POST /api/v1/orders/{id}/assign` | `TestAssignMechanic_WorkloadLimit` |
| **HU-03** | Diagnosis | INV-03 (State Machine), INV-05 | `POST /api/v1/orders/{id}/diagnosis` | `TestDiagnosis_AdvancesToInRepair` |
| **HU-04** | Intervention, Part | INV-04 (Immutability check) | `POST /api/v1/orders/{id}/interventions` | `TestIntervention_LogsLaborAndParts` |
| **HU-05** | ServiceOrder | INV-03, INV-04 (Delivery Lock) | `POST /api/v1/orders/{id}/deliver` | `TestDeliverOrder_PreventsSubsequentEdits` |
| **HU-06** | Warranty | INV-06 (Determinism) | `GET /api/v1/warranties/{id}` | `TestWarranty_CalculatesExpirationDate` |

## Traceability Principles

1. **Bi-directional Verification:** Every user story links to at least one domain invariant.
2. **Automated Validation:** Every invariant must have an automated test in Go (`internal/domain`).
3. **No Phantom Endpoints:** No production API route exists without an explicit user story.

## Verification Coverage

100% of functional user stories map directly to at least one unit/integration test in Go.

---

**Related:** [`user-stories.md`](./user-stories.md) · [`non-functional.md`](./non-functional.md)\n