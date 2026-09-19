# 00 — Engineering Governance

The governance section defines the organizational framework, engineering standards,
quality gates, and operational policies governing the development of the Automotive
Technical Support Management System.

## Contents

| Document | Purpose |
|---|---|
| [`agile-conventions.md`](./agile-conventions.md) | Sprint cycles, ceremonies, and task estimation for workshop features |
| [`definition-of-done.md`](./definition-of-done.md) | Explicit quality checklist for merging code and domain models |
| [`definition-of-ready.md`](./definition-of-ready.md) | Prerequisites before taking a workshop user story into a sprint |
| [`documentation-rules.md`](./documentation-rules.md) | Format rules, line constraints (25-80 lines), and review lifecycle |
| [`git-conventions.md`](./git-conventions.md) | Branching strategy, Conventional Commits, and PR review policy |
| [`security-policy.md`](./security-policy.md) | OWASP defense-in-depth, RBAC, JWT lifecycles, and SQL safety |
| [`_template-sprint-retro.md`](./_template-sprint-retro.md) | Standard retrospective template for continuous team improvement |

## Core Governance Principles

1. **Deterministic Quality:** No code reaches main without passing automated tests.
2. **Zero Security Neglect:** Security invariants are enforced at backend domain layers.
3. **Traceability:** Every architectural change links back to an agreed requirement.
4. **Living Documentation:** Specifications and code evolve together in every pull request.
5. **Operational Accountability:** Ownership of repair records and invariant verification.

---

**Related:** [`definition-of-done.md`](./definition-of-done.md) · [`documentation-rules.md`](./documentation-rules.md)\n