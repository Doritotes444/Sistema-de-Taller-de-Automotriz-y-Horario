# 06 — Data Architecture

The data architecture section documents database naming conventions, DDL migration
strategies, relational entity models, and initial seed datasets for MySQL 8.4.

## Contents

| Document | Purpose |
|---|---|
| [`database-conventions.md`](./database-conventions.md) | Table, column, index, and constraint naming standards |
| [`models.md`](./models.md) | Physical relational schema, data types, and foreign key relations |
| [`migrations.md`](./migrations.md) | DDL versioning, idempotent execution, and rollback strategies |

## Relational Architecture Overview

```mermaid
erDiagram
    CUSTOMERS ||--o{ VEHICLES : owns
    VEHICLES ||--o{ SERVICE_ORDERS : undergoes
    SERVICE_ORDERS ||--o{ DIAGNOSES : contains
    SERVICE_ORDERS ||--o{ INTERVENTIONS : records
    INTERVENTIONS ||--o{ WARRANTIES : generates
    USERS ||--o{ SERVICE_ORDERS : assigned_to
```

Persistence is managed with **MySQL 8.4 InnoDB**, enforcing strict referential integrity.

---

**Related:** [`models.md`](./models.md) · [`database-conventions.md`](./database-conventions.md)\n