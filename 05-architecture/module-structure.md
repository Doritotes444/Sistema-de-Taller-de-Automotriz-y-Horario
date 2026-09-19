# 05 — Module & Package Structure

This document outlines the package organization of the Go 1.25 backend codebase.

## Directory Tree

```
backend/
├── cmd/
│   └── api/
│       └── main.go          # Application bootstrap & dependency injection
├── internal/
│   ├── domain/              # Core business entities & invariants
│   │   ├── order.go
│   │   ├── vehicle.go
│   │   └── user.go
│   ├── usecase/             # Application workflows & transactions
│   │   ├── order_usecase.go
│   │   └── vehicle_usecase.go
│   ├── repository/          # MySQL database adapters
│   │   ├── mysql_order.go
│   │   └── mysql_vehicle.go
│   └── transport/
│       └── http/            # REST handlers & middlewares
│           ├── handler.go
│           └── middleware.go
└── go.mod
```

All application dependencies are wired explicitly in `cmd/api/main.go`.

---

**Related:** [`layered-architecture.md`](./layered-architecture.md) · [`boundary-enforcement.md`](./boundary-enforcement.md)\n