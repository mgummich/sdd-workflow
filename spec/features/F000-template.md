---
id: F000
status: draft        # draft | approved | done
complexity: L1       # L1 trivial | L2 normal | L3 architecture-impact
architectureImpact: false
---

# F000: [Feature Name]

## Intent

What should this feature achieve from a business perspective? Which user/problem does it serve? 1–3 sentences.

## Scope

**In scope:**
- …

**Out of scope:**
- …

## Business Rules

- Rule 1: …
- Rule 2: …

## Contracts

### API (if applicable)

- **Endpoint**: `METHOD /path`
- **Request**: structure + required fields
- **Response (200)**: structure
- **Error cases**: codes + structures (e.g., 400/401/404)

### Data model (if applicable)

- Entity: `User`
  - Fields: id, email, …
  - Invariants: email must be unique, etc.

## Scenarios

1. **[Title]**
   Given …
   When …
   Then …

2. **[Title]**
   Given …
   When …
   Then …

## Acceptance Criteria

- [ ] Critical business rules covered by automated tests
- [ ] All Contracts implemented exactly as specified
- [ ] All scenarios implemented as automated tests
- [ ] Negative paths (errors, edge cases) tested
- [ ] Security: no new secrets committed, inputs validated at boundaries
- [ ] Performance: no regression vs baseline (where applicable)

## Implementation Notes (optional)

Technical constraints, migration notes, known limitations.

## Progress

> Appended during implementation. Append-only.

<!--
**YYYY-MM-DD**
- Done: …
- Files: …
- Decision: … (only if non-obvious)
- Next: …
-->
