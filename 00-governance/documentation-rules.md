# 00 — Documentation Rules

> [!NOTE] GOVERNANCE POLICY
> This document defines the engineering standards and quality rules for all Software
> Design Documentation (SDD) in the Automotive Technical Support Management System.

## Document Lifecycle

| State | Definition | Marker |
|---|---|---|
| Draft | Content being formulated, pending technical verification | Open PR |
| Done | Peer-reviewed, verified against code, and merged to main | Instruction-free |

## Core Documentation Rules

1. **One Question Per Document:** Each document focuses on a single architectural topic.
2. **Strict Line Limits (25-80 lines):**
   - Files with fewer than 25 lines are incomplete stubs.
   - Files exceeding 80 lines contain multiple concerns and must be split.
3. **Structured Formats Over Prose:** Use Markdown tables, Mermaid diagrams, and code snippets.
4. **Falsifiable Statements:** Every requirement and non-functional metric must be measurable.
5. **Relative Hyperlinks:** Always link files using relative paths for portable navigation.
6. **English Standard:** All technical documentation is written in English for consistency.

## Automated Verification

Compliance is verified using automated repository linters that assert:
- Line counts strictly between 25 and 80.
- Zero unresolved template instruction markers.
- Valid Markdown table formats and rendering syntax.

---

**Related:** [`definition-of-done.md`](./definition-of-done.md) · [`README.md`](./README.md)\n