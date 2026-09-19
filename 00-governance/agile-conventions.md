# 00 — Agile Conventions

This document formalizes the agile working agreements for developing and maintaining
the Automotive Technical Support Management System.

## Sprint Cadence & Ceremonies

The team works in **two-week sprints** focusing on end-to-end workshop capabilities.

| Ceremony | Cadence | Duration | Purpose |
|---|---|---|---|
| Sprint Planning | Day 1 of sprint | 1 hour | Select and commit backlog items based on DoR |
| Daily Standup | Mon-Fri | 15 mins | Sync on progress, blockers, and mechanic invariants |
| Sprint Review | Final day | 45 mins | Live demo of working software to workshop stakeholders |
| Retrospective | Final day | 30 mins | Team inspection and continuous process improvement |

## Estimation & Capacity

- **Estimation Units:** Modified Fibonacci story points (1, 2, 3, 5, 8, 13).
- **Point Baseline:** 1 point = straightforward database migration or UI label tweak.
- **Complexity Ceiling:** Stories above 8 points must be sliced before sprint entry.

## Board Columns & WIP Limits

| Column | WIP Limit | Description |
|---|---|---|
| Backlog | No limit | Prioritized user stories ready for refinement |
| Ready | 6 | Stories meeting the Definition of Ready |
| In Progress | 4 | Actively developed by pairs or individual engineers |
| In Review | 3 | Pull requests awaiting peer review and test CI run |
| Done | No limit | Merged to main, meeting the Definition of Done |

---

**Related:** [`definition-of-ready.md`](./definition-of-ready.md) · [`definition-of-done.md`](./definition-of-done.md)\n