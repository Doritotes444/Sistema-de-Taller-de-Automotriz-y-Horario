# 01 — Glossary & Ubiquitous Language

This glossary defines standard terminology used across documentation, domain code,
database schemas, and UI components in the Automotive Workshop System.

## Domain Terms

| Term | Definition |
|---|---|
| **VIN** | Vehicle Identification Number: unique 17-character alphanumeric vehicle identifier |
| **Plate** | Unique official governmental vehicle license plate registration string |
| **Service Order** | Work ticket tracking a vehicle from reception through repair to delivery |
| **Diagnosis** | Technical assessment conducted by a mechanic identifying root defects and parts |
| **Intervention** | Concrete physical labor performed on the vehicle with recorded labor hours |
| **Spare Part** | Physical replacement component installed during a technical intervention |
| **Warranty** | Contractual coverage guarantee (Labor or Part) with a defined validity period |
| **Clinical Timeline** | Unified chronological timeline consolidating all events for a vehicle |
| **Technician (Mechanic)** | Skilled technical personnel responsible for diagnosis and repair |
| **Workshop Manager (Admin)** | Operational administrator overseeing reception, dispatch, and quality |

## State Machine Terms

- **RECEIVED:** Vehicle checked in at workshop; initial failure recorded.
- **IN_DIAGNOSIS:** Assigned technician inspecting vehicle and preparing diagnosis.
- **IN_REPAIR:** Technician actively executing physical repairs and interventions.
- **READY:** Work completed, verified, and vehicle ready for customer pickup.
- **DELIVERED:** Vehicle handed over to customer; order permanently closed.

---

**Related:** [`02-domain/entities-and-rules.md`](../02-domain/entities-and-rules.md)\n