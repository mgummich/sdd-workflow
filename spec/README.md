# Spec-Driven Development (SDD) Skeleton

This is your minimal, token-optimized SDD skeleton for consistent projects.

## Files

### Core (always present)
- `spec/00-constitution.md` – Project-wide principles and rules
- `spec/01-rules-llm.md` – Token-optimized rules for LLMs
- `spec/STATE.md` – Context compression between work waves

### Templates
- `spec/features/F000-template.md` – Feature spec template
- `spec/adr/ADR-000-template.md` – Architecture decision template

## How to use

### For a new project
1. Copy this skeleton into your repo.
2. Update `spec/00-constitution.md` with your project's tech stack and rules.
3. Update `spec/01-rules-llm.md` to reference your existing skills (BMAD, SDD-methodology, etc.).

### For each new feature
1. Copy `spec/features/F000-template.md` → `spec/features/F001-<feature-name>.md`.
2. Fill in Intent, Scope, Business Rules, Contracts, Scenarios, Acceptance Criteria.
3. Set `complexity`:
   - `L1` = trivial (no ADR needed)
   - `L2` = normal (ADRs if architecture affected)
   - `L3` = architecture-impact (ADR required)

### When working with LLMs
Minimal context per task:
```
Load:
- spec/01-rules-llm.md
- spec/features/F001-<feature-name>.md
(Optionally: 1–2 architecture/ADR files if architectureImpact: true)
```

## Token optimization checklist

Before each LLM-assisted task:
- ✅ Use spec-first (rules + 1 spec) instead of full repo
- ✅ Search before loading files
- ✅ Use outline instead of full file where possible
- ✅ Produce structured output (JSON, typed lists)
- ✅ Don't repeat rules in the prompt
- ✅ Compress context after each wave (update STATE.md)
- ✅ Cache/reuse existing artifacts instead of re-generating

## Workflow

Per feature: `Spec → Plan → Tasks → Implement → Verify`

- **Spec**: `spec/features/Fxxx-*.md`
- **Plan**: optional plan.md with task breakdown
- **Tasks**: tasks.md with checkboxes and acceptance criteria
- **Implement**: code + tests following the spec
- **Verify**: CI passes, tests cover all acceptance criteria

## Architecture decisions

ADRs only for:
- Hard to reverse
- Long-term impact
- Multiple teams/domains

Format: `spec/adr/ADR-xxx-*.md` (1–2 pages, status: Proposed → Accepted → Deprecated/Superseded)
