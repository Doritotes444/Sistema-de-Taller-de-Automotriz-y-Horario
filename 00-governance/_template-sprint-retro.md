# 00 — Sprint Retrospective Template

Use this template at the conclusion of each two-week sprint to document team insights,
process bottlenecks, and actionable improvements.

## Sprint Metadata

- **Sprint Number:** Sprint [X]
- **Date:** [YYYY-MM-DD]
- **Facilitator:** [Name / Role]
- **Participants:** [Team Members]

## Retrospective Categories

### 1. What Went Well (Praise & Successes)
- High test coverage on order state machine transitions.
- Clean Go Clean Architecture boundary separation.
- Rapid Docker container local boot times.

### 2. What Could Be Improved (Pains & Friction)
- Initial ambiguity in mechanic workload business rules.
- Test flakiness due to shared database test state.

### 3. Action Items (SMART Commitments)

| Action Item | Assignee | Target Date | Success Measure |
|---|---|---|---|
| Implement test transaction rollback helper | Lead Engineer | Next sprint | Zero database pollution in tests |
| Add documentation linter to pre-commit hook | DevOps Engineer | Next sprint | 100% pass on SDD line counts |

---

**Related:** [`agile-conventions.md`](./agile-conventions.md) · [`definition-of-done.md`](./definition-of-done.md)\n