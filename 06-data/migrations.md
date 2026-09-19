# 06 — Database Migrations & Seed Data

This document defines the migration and database initialization strategy for the
Automotive Workshop System.

## Migration Strategy

The system uses declarative, forward-only DDL scripts loaded automatically by
Docker Compose at database container startup from `database/init/01_init.sql`.

## Migration Lifecycle

```
01_init.sql (DDL + Tables + Indexes + Foreign Keys)
      |
      v
02_seed.sql (Bootstrap Administrator + Initial Technicians + Sample Vehicles)
```

## Idempotency & Safety Rules

- DDL scripts use `CREATE TABLE IF NOT EXISTS`.
- Seed data uses `INSERT IGNORE` or `ON DUPLICATE KEY UPDATE` to avoid collision on restarts.
- Destructive operations (`DROP TABLE`) are strictly forbidden in production migrations.

## Rollback Procedures

- In development, `docker compose down -v` flushes the MySQL volume for a clean rebuild.
- In production, backward-compatible alter table migrations are drafted and tested on staging.

---

**Related:** [`models.md`](./models.md) · [`database-conventions.md`](./database-conventions.md)\n