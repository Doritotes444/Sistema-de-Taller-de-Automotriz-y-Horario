# 00 — Security Policy

This document defines the defense-in-depth security standards implemented across
the Automotive Technical Support Management System.

## Threat Model & OWASP Top 10 Protections

| OWASP Risk | Applied Defense in Automotive System |
|---|---|
| **A01: Broken Access Control** | Backend role validation on all endpoints; strict ownership checks on orders |
| **A02: Cryptographic Failures** | Passwords hashed with bcrypt (cost 12); JWT signed with HS256 and secret keys |
| **A03: Injection** | Strict parameterized SQL queries in Go database repository; zero string concatenation |
| **A07: Identification Failures** | Stateless JWT tokens with 24-hour expiration; rate limiting on authentication routes |
| **A09: Logging & Monitoring** | Critical audit events logged with actor ID, timestamp, and entity mutations |

## Role-Based Access Control (RBAC)

The system enforces two distinct roles at the backend transport middleware:

- **Administrator (Workshop Manager):** Full CRUD permissions on vehicles, customers,
  users, warranties, and mechanic assignments.
- **Technician (Mechanic):** Restricted to viewing assigned orders, creating diagnoses,
  registering interventions, and advancing status from `IN_DIAGNOSIS` to `READY`.

## Sensitive Data Handling

- Customer phone numbers and emails are protected against unauthenticated scraping.
- Plaintext passwords never leave the client and are never written to logs or database.

---

**Related:** [`definition-of-done.md`](./definition-of-done.md) · [`README.md`](./README.md)\n