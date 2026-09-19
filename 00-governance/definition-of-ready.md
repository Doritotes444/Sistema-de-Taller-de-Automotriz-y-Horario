# 00 — Definition of Ready (DoR)

A backlog item or user story is considered **Ready** for sprint planning only when
it meets all criteria below, preventing ambiguous requirements from entering development.

## Quality Criteria for Backlog Items

| Criterion | Requirement | Verification |
|---|---|---|
| **Clear Persona** | Specifies whether actor is Administrator or Technician | Role identified in title/header |
| **Business Context** | Explains the operational workshop problem being solved | Problem statement in description |
| **Acceptance Criteria** | Written in Given-When-Then (Gherkin) format | Testable acceptance criteria |
| **Domain Invariants** | Explicitly defines relevant state machine or workload constraints | Business rules section |
| **API Contract** | Request/response JSON payloads defined | Draft schema or endpoint path |
| **Data Impact** | Affected tables and columns identified | Schema review completed |
| **Estimation** | Sized by the development team | Story points assigned (<= 8) |

## DoR Verification Checklist

- [ ] Acceptance criteria can be validated with an automated test case.
- [ ] Dependencies on external modules or migrations are identified.
- [ ] UI mockups or layout wireframes are attached if frontend changes are involved.
- [ ] No unanswered business questions or ambiguous failure conditions remain.

---

**Related:** [`agile-conventions.md`](./agile-conventions.md) · [`definition-of-done.md`](./definition-of-done.md)\n