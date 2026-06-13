# 01 — Rules for LLMs

> Provider-agnostic. Runtime-specific rules: see `CLAUDE.md` or `AGENTS.md`.

## 1. Role

Turn specs into code and tests. Specs are the source of truth. If something is unclear or contradictory, **ask** — never guess.

## 2. Session start

1. Read `spec/STATE.md`.
2. If `active_feature` is set, read every path in `load:` before doing anything else.
3. Keep the active feature spec pinned in working memory for the whole session.

If `active_feature: null`, ask which feature to work on.

## 3. Load discipline

- Load `spec/01-rules-llm.md` + the active feature spec. Read other files only when the current task demands it.
- Search/snippets before full reads.
- Never load the full repo by default.

## 4. Status gate

```
draft → approved → done
```

- `draft` — refine the spec only. No implementation.
- `approved` — implementation green-lit.
- `done` — read-only.

Check status before writing any code.

## 5. Spec-delta gate

If implementation reveals a spec gap or contradiction:

1. Stop.
2. Propose a **spec delta** — concrete changes to the spec sections that need updating.
3. Wait for the human to update the spec.
4. Resume.

## 6. Working with feature specs

Feature specs always have:

`Intent`, `Scope`, `Business Rules`, `Contracts`, `Scenarios`, `Acceptance Criteria`, optional `Implementation Notes`, `Progress`.

Process:

1. Understand Intent, Scope, Business Rules, Contracts.
2. Derive tests from Scenarios + AC.
3. Implement to satisfy those tests without breaking Contracts.

## 7. Code & tests

- Stack defined per project (see constitution).
- Code: small, well-named, testable; side effects (I/O, network) isolated.
- Tests: cover every AC, names describe observable behavior, write tests first.

## 8. Output format

- Structured output (JSON, typed lists) over prose.
- Example: `{"files": ["auth.ts", "login.ts"]}` not "I think we should create three files…".

## 9. Hard rules

- No changes to Contracts without a spec delta.
- No new frameworks/libraries not already in the project.
- No "optimizations" that violate the spec.

## 10. End of session

1. Append a Progress entry to the active feature spec:

   ```markdown
   **YYYY-MM-DD**
   - Done: …
   - Files: …
   - Decision: … (only if non-obvious)
   - Next: …
   ```

2. Update `spec/STATE.md` if the active feature changed.
3. On verify: append `Verified. AC all green.`, flip `status: done`.
