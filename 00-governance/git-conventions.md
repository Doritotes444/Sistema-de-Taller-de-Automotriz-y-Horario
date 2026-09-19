# 00 — Git Conventions

This document establishes the version control workflow, branch naming conventions,
and commit formatting rules for the Automotive Workshop codebase.

## Branching Model

The project follows a **Trunk-Based Development** model with short-lived feature branches:

```
main  ======================================================> (stable, deployable)
         \-- feat/mechanic-workload-limit ---/
         \-- fix/order-state-transition -----/
```

## Branch Naming Rules

- `feat/<short-description>`: New capability or domain module.
- `fix/<issue-description>`: Bug remediation or invariant correction.
- `test/<test-scope>`: Automated test suites and regression tests.
- `docs/<sdd-section>`: Documentation updates and SDD compliance.

## Conventional Commits

Commit messages follow the Conventional Commits specification:

```
<type>(<scope>): <short imperative summary>

[optional detailed description explaining why the change was made]
```

| Type | When to Use | Example |
|---|---|---|
| `feat` | New operational functionality | `feat(order): add atomic transition validator` |
| `fix` | Defect remediation | `fix(auth): prevent mechanic token privilege escalation` |
| `test` | Adding or updating tests | `test(domain): verify single active order per mechanic` |
| `docs` | SDD documentation changes | `docs(sdd): update 05-architecture ADR-002` |

---

**Related:** [`agile-conventions.md`](./agile-conventions.md) · [`definition-of-done.md`](./definition-of-done.md)\n