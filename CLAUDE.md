<!-- Sibling: AGENTS.md (Codex). Keep SDD rules in sync. -->

# SDD Workflow (Claude Code)

Specs are the source of truth. If code conflicts with a spec, the spec wins. If unclear, ask.

## First action — every session

1. Read `spec/STATE.md`
2. If `active_feature` is set: read every path in `load:` before doing anything else
3. Keep the active feature spec pinned in working memory for the whole session

If `active_feature: null`, ask which feature to start.

## Workflow

```
Spec → Approve → Implement → Verify
```

Status: `draft → approved → done`.

- **Status gate:** no code until `status: approved`. In `draft`, help refine the spec only.
- **Spec-delta gate:** if implementation reveals a spec gap, stop. Propose a spec delta. Do not silently patch behavior. Resume only after the human updates the spec.

## Tools

- Prefer `Grep`/`Glob` to find code before `Read`
- Read snippets (line ranges), not whole files
- Structured output (JSON, typed lists) over prose

## End of session

1. Append a Progress entry to the active feature spec:

   ```markdown
   **YYYY-MM-DD**
   - Done: …
   - Files: …
   - Decision: … (only if non-obvious)
   - Next: …
   ```

2. Update `spec/STATE.md` if the active feature changed.

Full rules: `spec/01-rules-llm.md`.
