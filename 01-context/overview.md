# 01 — System Overview

The Automotive Technical Support Management System is a specialized web platform
engineered to eliminate operational chaos, paper-based tracking, and unbilled repairs
in modern automotive mechanical workshops.

## Operational Problem Statement

Automotive repair centers face severe operational friction:
1. **Paper-Based Inefficiency:** Work orders written on paper get lost between desks.
2. **Technician Idle Time & Overload:** Disorganized dispatch leads to uneven workload.
3. **Unrecorded Interventions:** Technicians replace components without billing records.
4. **Customer Uncertainty:** Vehicle owners lack visibility into real-time repair progress.
5. **Warranty Disputes:** Difficulties verifying whether a broken part is under active warranty.

## Core Solution Architecture

The platform provides a centralized, deterministic modular monolith:
- **Vehicle & Customer Registry:** Immediate lookup by plate or VIN.
- **Automated Lifecycle Enforcement:** Linear state transitions with timestamps.
- **Mechanic Workload Invariant:** Ensures technicians focus on one active order at a time.
- **Unified Clinical History:** Full vehicular pedigree for lifecycle transparency.

```
Reception -> Order Created -> Assigned -> Diagnosed -> Repaired -> Ready -> Delivered
```

---

**Related:** [`scope.md`](./scope.md) · [`03-product/vision.md`](../03-product/vision.md)\n