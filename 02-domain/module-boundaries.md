# 02 — Module Boundaries

This document defines boundary rules ensuring that subdomains remain decoupled within
the Go modular monolith codebase.

## Boundary Principles

```
[Transport: HTTP] ---> [Use Cases] ---> [Domain Entities]
                              |
                              v
                   [Repository Interface] <--- [MySQL Adapter]
```

1. **Domain Isolation:** The `internal/domain` package has zero external imports
   (no SQL packages, no HTTP frameworks, no external libraries).
2. **Interface Inversion:** Use cases interact with persistence through repository
   interfaces defined at the application boundary.
3. **Transactional Encapsulation:** Order state mutations and mechanic assignment
   validation occur within single database transactions.

## Communication Matrix Between Modules

| Caller | Target | Permitted Mechanism |
|---|---|---|
| Transport | Use Case | Direct method invocation via interface |
| Use Case | Domain | Pure struct construction and method validation |
| Use Case | Repository | Go interface method call |
| Repository | Database | Standard library `database/sql` queries |

Direct cross-module database writes outside domain use cases are strictly forbidden.

---

**Related:** [`05-architecture/boundary-enforcement.md`](../05-architecture/boundary-enforcement.md)\n