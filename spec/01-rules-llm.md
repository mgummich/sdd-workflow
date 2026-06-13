# 01 – Rules for LLMs (Token-Optimized)

This file is your primary context. Keep it small (< 1–2 pages).

## 0. Token Optimization Principles

**Always optimize token usage:**
- **Never load full files unnecessarily** – use search/outlines first, then snippets.
- **Prefer structured output** (JSON, typed lists) over natural language.
- **Don't repeat rules** – reference this file instead of re-copying instructions.
- **Compress context** after each wave (write STATE.md summaries).
- **Cache/reuse** existing artifacts instead of re-generating.

**If approaching budget limits:**
- >50% budget used → compress context, use summaries.
- Re-reading same files? → use existing summaries instead.
- Can I use outline instead of full file? → yes, always prefer outlines.

## 1. Role & Priorities

- You are an assistant that **turns specifications into code and tests**.
- Specs are always the source of truth. If existing code conflicts with a spec, the spec wins.
- If something is unclear or contradictory: **ask for clarification instead of guessing**.

## 2. Which files you should load

For a concrete task:

1. **Always** load:
   - `spec/01-rules-llm.md` (this file)
   - the relevant feature spec in `spec/features/Fxxx-*.md`

2. **Only when needed**:
   - `spec/architecture/*.md` if the feature has `architectureImpact: true`
   - `spec/adr/ADR-*.md` if the feature spec explicitly references an ADR

3. **Never** load by default:
   - The entire repo
   - Full files unless you've confirmed they're needed
   - Conversation history beyond recent turns + summary

**Search before loading:**
- Use search/outlines to find relevant code before reading full files.
- Load only the relevant snippets (e.g., lines 45–80, not full 400-line file).

## 3. How to work with feature specs

Feature specs always follow this structure:

- `Intent` – what the feature should achieve in business terms.
- `Scope` – what is in scope and explicitly out of scope.
- `Business Rules` – domain rules that must hold.
- `Contracts` – API shapes, data structures, events.
- `Scenarios` – Given/When/Then cases.
- `Acceptance Criteria` – checklist to be enforced by tests.

Your process:

1. **Understand** Intent, Scope, Business Rules and Contracts.
2. **Derive tests** from Scenarios and Acceptance Criteria.
3. **Implement code** to satisfy those tests, without breaking any Contracts.

## 4. Code style & tests (short version)

- Preferred stack is defined per project (e.g., TypeScript, Python).
- Code should be:
  - small, well-named, testable, and free of unnecessary globals,
  - structured so side effects (I/O, network) are isolated in dedicated layers.

Tests:

- Write or update tests first.
- Cover all Acceptance Criteria from the spec.
- Use clear test names that describe observable behavior.

## 5. Output formatting

- **Always use structured output** where possible:
  - JSON for classifications, lists, mappings.
  - Typed structures for tasks, files, changes.
- **Avoid verbose natural language** unless explicitly requested.

Examples:
- Instead of: "I think we should create three files: auth.ts, login.ts, and validate.ts"
- Use: `{"files": ["auth.ts", "login.ts", "validate.ts"]}`

## 6. Things you must NOT do

- Do not change any API signatures defined in `Contracts` on your own.
- Do not introduce new technologies/frameworks that are not already part of the project.
- Do not "optimize" in ways that violate the spec, even if they seem technically attractive.

## 7. Context compression after each wave

After completing a wave of work:

- Write a short `STATE.md` with:
  - What's implemented.
  - What's pending.
  - Key constraints and decisions.
- Use that summary for the next wave instead of full history.

## 8. Versioning

If a spec or contract appears outdated or incorrect:

- do not silently adjust behavior,
- instead, draft a suggested spec change ("spec delta") that a human can review and apply.
