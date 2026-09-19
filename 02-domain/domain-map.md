# 02 — Domain Map

The Automotive Workshop domain is partitioned into distinct subdomains, ensuring high
cohesion within business concepts and loose coupling across components.

## Subdomain Decomposition

```mermaid
flowchart TD
    Registry["Vehicle & Customer Registry<br/>(Core Domain)"]
    Operations["Service Order Operations<br/>(Core Domain)"]
    Technical["Diagnosis & Interventions<br/>(Core Domain)"]
    Warranties["Warranty Management<br/>(Supporting Domain)"]
    Clinical["Vehicular History & Timeline<br/>(Supporting Domain)"]
    IAM["Identity & Access Management<br/>(Generic Subdomain)"]

    Registry --> Operations
    Operations --> Technical
    Technical --> Warranties
    Operations --> Clinical
    Technical --> Clinical
    IAM -.-> Operations
```

## Subdomain Descriptions

- **Vehicle & Customer Registry:** Owns master records for customers and vehicles.
- **Service Order Operations:** Manages work tickets and lifecycle state transitions.
- **Diagnosis & Interventions:** Captures technical findings, labor, and spare parts.
- **Warranty Management:** Computes coverage validity based on intervention dates.
- **Vehicular History & Timeline:** Assembles immutable chronological activity records.
- **IAM:** Handles user credentials, password hashing, and role-based authorization.

---

**Related:** [`entities-and-rules.md`](./entities-and-rules.md) · [`module-boundaries.md`](./module-boundaries.md)\n