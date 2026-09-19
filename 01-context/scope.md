# 01 — System Scope

This document defines the functional boundaries of the MVP and distinguishes
in-scope capabilities from external corporate integrations.

## In-Scope Capabilities (MVP)

- **Authentication & Roles:** JWT session authentication for Admin and Technician.
- **Customer & Vehicle Management:** Full registration with unique VIN and license plate.
- **Service Order Lifecycle:** Strict state transitions:
  `RECEIVED -> IN_DIAGNOSIS -> IN_REPAIR -> READY -> DELIVERED`.
- **Technician Dispatch:** Manager assigns available mechanics under workload invariants.
- **Technical Diagnosis:** Recording findings and recommended component repairs.
- **Intervention & Parts Tracking:** Logging labor hours and spare parts used.
- **Warranty Issuance & Verification:** Tracking LABOR and PART warranty validity.
- **Vehicular Clinical Timeline:** Searchable chronological history per vehicle.

## Out-of-Scope Capabilities (Deferred)

| Out-of-Scope Area | Rationale | Handled By |
|---|---|---|
| **Online Billing & Invoicing** | Workshop charges at physical cashier desk | Corporate ERP / POS |
| **External Supplier Inventory** | Complex supply chain purchasing is out of MVP | External ERP |
| **Automated SMS Notifications** | Cost and third-party telecom dependency | Future phase |
| **Predictive AI Maintenance** | Requires large historical telematics datasets | Future phase |

---

**Related:** [`overview.md`](./overview.md) · [`03-product/problem-framing.md`](../03-product/problem-framing.md)\n