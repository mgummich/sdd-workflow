# F000: [Feature Name]

metadata:
  id: F000
  status: draft        # draft | approved | implemented
  complexity: L1       # L1 = trivial, L2 = normal, L3 = architecture-impact
  architectureImpact: false

## Intent

Short description in 1–3 sentences:
- What should this feature achieve from a business perspective?
- Which user/problem does it serve?

## Scope

- In scope:
  - Item 1
  - Item 2
- Out of scope:
  - Item A
  - Item B

## Business Rules

- Rule 1: …
- Rule 2: …
- Rule 3: …

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

## Scenarios (Given / When / Then)

1. Scenario 1 – [Title]  
   Given …  
   When …  
   Then …

2. Scenario 2 – [Title]  
   Given …  
   When …  
   Then …

## Acceptance Criteria (Checklist)

- [ ] Critical business rules are covered by automated tests.
- [ ] All `Contracts` are implemented exactly as specified.
- [ ] All scenarios are implemented as automated tests.
- [ ] Negative paths (errors, edge cases) are tested.
- [ ] Logging/observability is present where needed.

## Implementation Notes (optional)

Only use this section for information that does not fit Business Rules/Contracts,
for example:
- technical constraints,
- migration notes,
- known limitations.
