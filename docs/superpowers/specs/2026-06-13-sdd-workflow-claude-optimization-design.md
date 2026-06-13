# SDD Workflow — Claude Optimization & Multi-Provider Support

**Date:** 2026-06-13

## 1. Goal

Minimal SDD template optimized for Claude Code, with Codex as a parallel option. Few files, few states, few rules; preserves spec-as-source-of-truth without process ceremony.

## 2. Non-goals

- Ralph-style autonomous loops
- Spec-as-source code generation
- Multi-state status machines, separate review docs, phase tables
- Custom orchestration tooling

---

## 3. Final structure

```
sdd-workflow/
├── README.md                              # already exists
├── CLAUDE.md                              # NEW — Claude Code bootstrap
├── AGENTS.md                              # NEW — Codex bootstrap
├── .github/                               # already exists
└── spec/
    ├── README.md                          # EDIT
    ├── 00-constitution.md                 # EDIT
    ├── 01-rules-llm.md                    # EDIT
    ├── STATE.md                           # EDIT — minimal YAML pointer
    ├── features/F000-template.md          # EDIT — status + Progress log
    └── adr/ADR-000-template.md            # unchanged
```

---

## 4. Workflow

```
Spec → Approve → Implement → Verify
```

Single flow. Complexity is metadata, not a different process. ADRs only when an architecture decision warrants one.

### 4.1 Status

```
draft → approved → done
```

- `draft` — being written or reviewed
- `approved` — implementation green-lit
- `done` — AC all green, verified

Review is a conversation, not a state. Findings refine the spec directly before `draft → approved`.

### 4.2 Spec-delta gate

When implementation reveals a spec gap:

1. Stop.
2. Propose spec delta in the response (or as a PR comment).
3. Human updates spec.
4. Resume.

No silent patches. This is the only enforced gate.

---

## 5. Context architecture

### 5.1 STATE.md — pointer

```yaml
---
active_feature: F001
load:
  - spec/01-rules-llm.md
  - spec/features/F001-auth.md
---
```

When idle:

```yaml
---
active_feature: null
---
```

### 5.2 Per-feature Progress log

Appended to each `Fxxx-*.md` as work proceeds:

```markdown
## Progress

**2026-06-13**
- Done: Added rate limiter to auth middleware
- Files: `src/auth.ts`, `tests/auth.test.ts`
- Decision: Used existing Redis client; no new deps
- Next: Wire rate limiter into login endpoint
```

Rules:
- Append-only.
- Date as section header.
- Max 5 lines per entry.
- On verify: final entry says `Verified. AC all green.`

### 5.3 Session start

1. Read `spec/STATE.md` → get `active_feature` + `load` list
2. Read all files in `load`
3. Keep the feature spec pinned in working memory throughout the session

CLAUDE.md and AGENTS.md state this as the **first action** of any session.

### 5.4 Load discipline

> Load the active feature spec + `01-rules-llm.md`. Read other files only when the current task demands it. Prefer Grep/snippets over full files.

---

## 6. Provider files

### 6.1 CLAUDE.md

< 400 tokens. Contents:

1. **First action:** read `spec/STATE.md` → load files in `load:`
2. **Workflow:** `Spec → Approve → Implement → Verify`
3. **Status gate:** no code until `status: approved`
4. **Spec-delta gate:** stop if spec gap found; propose delta, don't patch
5. **Tool hints:** Read/Grep/Glob; snippets > full files; structured output
6. **End of session:** append Progress entry to active feature spec
7. **Defer to:** `spec/01-rules-llm.md` for full rules

### 6.2 AGENTS.md

Mirrors CLAUDE.md. Tool section is generic ("file read tools"). Footer: "Adjust tool names for your Codex runtime." Both files carry a sibling pointer at the top:

```markdown
<!-- Sibling: AGENTS.md (Codex). Keep SDD rules in sync. -->
```

Drift is acceptable for tool-specific details; SDD rules must match.

---

## 7. Feature spec template (F000)

```yaml
---
id: F000
status: draft        # draft | approved | done
complexity: L1       # L1 trivial | L2 normal | L3 architecture-impact
architectureImpact: false
---
```

Sections:

- `## Intent`
- `## Scope` (in / out)
- `## Business Rules`
- `## Contracts` (API, data model)
- `## Scenarios` (Given/When/Then)
- `## Acceptance Criteria` (checklist enforced by tests)
- `## Implementation Notes` (optional)
- `## Progress` (appended during implement)

---

## 8. Template-vs-instance rule

Added to `spec/README.md`:

> **When using as a template:**
> - Keep `F000-template.md` and `ADR-000-template.md` — they're stencils.
> - Reset `spec/STATE.md` to `active_feature: null`.
> - Replace `<TODO>` markers in `spec/00-constitution.md` with project specifics.
> - Pick provider files: CLAUDE.md, AGENTS.md, or both. Delete what you don't use.

---

## 9. Verify

1. Walk the spec's `## Acceptance Criteria` — every box checked.
2. Run tests; capture output.
3. Append final Progress entry: `Verified. AC all green.`
4. Flip `status: done`.

---

## 10. Token budgets

- `CLAUDE.md`: < 400 tokens
- `AGENTS.md`: < 400 tokens
- `STATE.md`: < 50 tokens
- `01-rules-llm.md`: < 1500 tokens

Trim before commit if exceeded.

---

## 11. Deferred

- Git integration of status transitions
- `## Anti-scope` section in feature template
- Multi-perspective review automation
- Complexity-specific workflow branches

---

## 12. Acceptance criteria

- [ ] `CLAUDE.md` and `AGENTS.md` exist at repo root per §6.
- [ ] `spec/STATE.md` matches the format in §5.1.
- [ ] `spec/features/F000-template.md` carries the frontmatter and section list from §7.
- [ ] `spec/00-constitution.md` defines the workflow (§4) and 3-state status (§4.1).
- [ ] `spec/01-rules-llm.md` is provider-agnostic and codifies the spec-delta gate.
- [ ] `spec/README.md` includes the template-vs-instance rule from §8.
- [ ] All token budgets in §10 hold.
