# 06 — Database Conventions

This document establishes relational design conventions and naming standards applied
across MySQL 8.4 database tables and columns.

## Naming Standards

| Element | Convention | Example |
|---|---|---|
| **Tables** | Lowercase plural snake_case | `service_orders`, `vehicles`, `users` |
| **Columns** | Lowercase snake_case | `reported_failure`, `entry_at`, `plate` |
| **Primary Keys** | Explicit `id` (INT AUTO_INCREMENT or UUID) | `id` |
| **Foreign Keys** | Singular referenced table name + `_id` | `vehicle_id`, `technician_id` |
| **Unique Indexes** | `uk_` + table + `_` + column | `uk_vehicles_plate`, `uk_vehicles_vin` |
| **Indexes** | `idx_` + table + `_` + column | `idx_orders_status`, `idx_orders_vehicle` |

## Storage Engine & Character Encoding

- **Storage Engine:** InnoDB (mandatory for transactional ACID guarantees).
- **Character Set:** `utf8mb4` with `utf8mb4_unicode_ci` collation.
- **Timestamps:** Handled as `DATETIME` or `TIMESTAMP` in UTC.

---

**Related:** [`models.md`](./models.md) · [`migrations.md`](./migrations.md)\n