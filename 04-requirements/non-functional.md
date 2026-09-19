# 04 — Non-Functional Requirements (NFRs)

This document formalizes measurable engineering constraints and performance metrics
for the Automotive Workshop System.

## Measurable NFR Metrics

| ID | Category | Requirement & Target Metric | Falsifiable Test |
|---|---|---|---|
| **NFR-01** | Performance | API endpoints respond with p95 < 200 ms under 50 concurrent requests | Load test via k6 or Go benchmark |
| **NFR-02** | Concurrency | Zero race conditions on technician assignment under concurrent dispatch | Parallel HTTP assignment test suite |
| **NFR-03** | Data Integrity | ACID compliance: order mutations and state transitions use transactions | Multi-statement MySQL rollback test |
| **NFR-04** | Security | Passwords hashed with bcrypt (cost 12); JWT expired in 24h | Token decode and crypto audit |
| **NFR-05** | Availability | System boots in < 15 seconds via Docker Compose | Automated container health check |
| **NFR-06** | Usability | SPA initial bundle loads in < 2.0 seconds on standard broadband | Lighthouse performance audit |

## Persistence & Integrity Constraints

- Database runs on **MySQL 8.4** with InnoDB storage engine.
- Strict foreign keys (`ON DELETE RESTRICT`) prevent orphaned repair records.
- Unique indexes enforce non-duplicate license plates and VINs globally.

---

**Related:** [`user-stories.md`](./user-stories.md) · [`05-architecture/layered-architecture.md`](../05-architecture/layered-architecture.md)\n