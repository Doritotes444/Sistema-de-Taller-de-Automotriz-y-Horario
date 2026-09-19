# 00 — Definition of Done (DoD)

A user story, feature, or bug fix for the Automotive Workshop System is considered
**Done** only when all conditions in this checklist are verified.

## Engineering Checklist

- [ ] **Clean Architecture Adherence:** Domain entities have zero external dependencies.
- [ ] **Business Invariants Verified:**
  - Mechanic workload constraint: technician has at most one active order.
  - State machine transitions: valid sequence `RECEIVED -> IN_DIAGNOSIS -> IN_REPAIR -> READY -> DELIVERED`.
  - Immutable delivered state: delivered orders cannot be edited or reopened.
- [ ] **Automated Testing:**
  - Unit tests cover domain logic and state transitions (`go test ./...`).
  - Integration tests verify SQL queries and transaction rollbacks.
  - Test suite passes 100% with zero flakes.
- [ ] **Database Integrity:**
  - DDL migrations execute cleanly and idempotently.
  - Foreign keys, unique indexes (VIN, Plate), and cascade rules are enforced.
- [ ] **Security & Code Quality:**
  - RBAC checked: only Administrators assign mechanics; Technicians edit assigned orders.
  - Input sanitization and parameterized queries prevent SQL injection.
  - Zero high/critical vulnerabilities reported by static analyzers.
- [ ] **Documentation:**
  - Relevant SDD documents updated in the same pull request.
  - Line count remains between 25 and 80 lines per document.
- [ ] **Peer Review:** Approved by at least one software engineer.

---

**Related:** [`definition-of-ready.md`](./definition-of-ready.md) · [`documentation-rules.md`](./documentation-rules.md)\n