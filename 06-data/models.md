# 06 — Relational Models & Schema

This document details the core database tables representing the domain model in MySQL 8.4.

## Table Definitions

### 1. `users`
- `id` (INT PK AUTO_INCREMENT)
- `username` (VARCHAR(50) UNIQUE NOT NULL)
- `password_hash` (VARCHAR(255) NOT NULL)
- `role` (ENUM('ADMIN', 'TECHNICIAN') NOT NULL)
- `active` (TINYINT(1) DEFAULT 1)

### 2. `customers`
- `id` (INT PK AUTO_INCREMENT)
- `name` (VARCHAR(100) NOT NULL)
- `phone` (VARCHAR(20) NOT NULL)
- `email` (VARCHAR(100) NOT NULL)

### 3. `vehicles`
- `id` (INT PK AUTO_INCREMENT)
- `customer_id` (INT FK -> customers.id)
- `plate` (VARCHAR(10) UNIQUE NOT NULL)
- `vin` (VARCHAR(17) UNIQUE NOT NULL)
- `brand` (VARCHAR(50) NOT NULL)
- `model` (VARCHAR(50) NOT NULL)
- `year` (INT NOT NULL)

### 4. `service_orders`
- `id` (INT PK AUTO_INCREMENT)
- `vehicle_id` (INT FK -> vehicles.id)
- `technician_id` (INT FK -> users.id NULLABLE)
- `status` (ENUM('RECEIVED','IN_DIAGNOSIS','IN_REPAIR','READY','DELIVERED'))
- `reported_failure` (TEXT NOT NULL)
- `entry_at` (DATETIME NOT NULL)
- `exit_at` (DATETIME NULL)

### 5. `diagnoses` & `interventions`
- `diagnoses`: `id`, `order_id`, `technician_id`, `findings`, `components_to_repair`.
- `interventions`: `id`, `order_id`, `description`, `labor_hours`, `parts_used`.
- `warranties`: `id`, `intervention_id`, `type` (`LABOR`/`PART`), `months`, `expires_at`.

---

**Related:** [`database-conventions.md`](./database-conventions.md) · [`migrations.md`](./migrations.md)\n