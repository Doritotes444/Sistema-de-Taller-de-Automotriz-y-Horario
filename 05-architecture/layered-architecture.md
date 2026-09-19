# 05 — Layered Clean Architecture

The Go backend adheres to **Clean Architecture**, organizing code into concentric rings
with the Dependency Inversion Principle.

## Layers Description

```
[Transport / HTTP]  ->  [Use Cases / Application]  ->  [Domain / Entities]
                                |
                                v
                     [Repository Interfaces]  <--  [MySQL Adapters]
```

1. **Domain Layer (`internal/domain`):** Pure business logic, entity structs, and
   invariants (e.g. state machine validators). Zero external imports.
2. **Use Case Layer (`internal/usecase`):** Application orchestration, transaction
   coordination, and authorization checks.
3. **Repository Layer (`internal/repository`):** SQL persistence adapters implementing
   use case repository interfaces using parameterized queries.
4. **Transport Layer (`internal/transport/http`):** HTTP handlers, JSON serialization,
   middleware authentication, and status code translation.

---

**Related:** [`boundary-enforcement.md`](./boundary-enforcement.md) · [`module-structure.md`](./module-structure.md)\n