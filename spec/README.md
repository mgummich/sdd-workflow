# Spec-Driven Development (SDD) Skeleton

Minimal SDD template for AI-assisted projects. Optimized for Claude Code; works with Codex via `AGENTS.md`.

## Files

### Core

- `spec/00-constitution.md` — project principles, workflow, status rules
- `spec/01-rules-llm.md` — provider-agnostic rules for LLMs
- `spec/STATE.md` — pointer to the active feature (what to load)

### Templates (stencils — copy, don't edit)

- `spec/features/F000-template.md` — feature spec template
- `spec/adr/ADR-000-template.md` — architecture decision template

### Provider bootstraps (root of repo)

- `CLAUDE.md` — Claude Code auto-loads this
- `AGENTS.md` — Codex auto-loads this

## Workflow

```
Spec → Approve → Implement → Verify
```

Status: `draft → approved → done`.

Per feature:

1. Copy `spec/features/F000-template.md` → `spec/features/F001-<name>.md`.
2. Fill Intent, Scope, Business Rules, Contracts, Scenarios, AC. Set `status: draft`.
3. Review with LLM and humans. Refine spec until ready. Flip `status: approved`.
4. Update `spec/STATE.md` — set `active_feature: F001` and `load:`.
5. Implement. LLM appends `## Progress` entries each session.
6. Verify: walk AC checklist, run tests, append final Progress entry, flip `status: done`.

ADRs: only for decisions that are hard to reverse, long-term impact, or span multiple teams.

## When using as a template

- Keep `F000-template.md` and `ADR-000-template.md` — they're stencils.
- Reset `spec/STATE.md` to `active_feature: null`.
- Replace `<TODO>` markers in `spec/00-constitution.md`.
- Pick provider files: `CLAUDE.md`, `AGENTS.md`, or both. Delete what you don't use.

## LLM context per task

Minimum:

```
spec/01-rules-llm.md
spec/features/F00x-<name>.md
```

Don't load the full repo. See `spec/01-rules-llm.md` for full rules.
