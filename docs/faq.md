# FAQ

## Why only 3 statuses?

`draft → approved → done` is enough to gate the things that matter: no code before approval, no premature "done." Adding `in_review`, `in_progress`, etc. duplicates state that's already obvious from the Progress log or the `load:` list in STATE.md.

## Why no `phase` field?

Status carries it. `draft` means you're refining the spec. `approved` means you're implementing or verifying. Adding a phase field is a second source of truth that drifts.

## Why is STATE.md just a pointer?

Per-feature history lives in the feature spec's `## Progress` log. That keeps state attached to the spec it describes. STATE.md only answers one question: "which feature is active right now?"

## Why two provider files instead of one?

Claude Code auto-loads `CLAUDE.md`. Codex auto-loads `AGENTS.md`. The auto-load convention is fixed by the tool, so we provide both. SDD rules are kept in sync between them; tool hints diverge per runtime.

## What if I use both Claude Code and Codex?

Keep both files. They share SDD rules. Each tool reads only its own bootstrap.

## How do I add a new agent (Cursor, Aider, …)?

Add a new top-level file (e.g., `.cursorrules`, `AIDER.md`) following whatever convention that tool uses. Mirror the SDD rules from `CLAUDE.md`. The `spec/` directory is provider-agnostic.

## Why no Plan step?

The original SDD pipeline had `Spec → Plan → Tasks → Implement → Verify`. We dropped Plan because for L1/L2 features it duplicates Tasks. For L3 (architecture-impact), the ADR is the plan.

## Why is the Progress log inside the feature spec, not separate?

Two reasons:
1. Loading the spec loads the history. Single file, full context.
2. Append-only logs are easier to write correctly than separate state files that have to be kept in sync.

## What's a "spec delta"?

A concrete proposed change to one or more sections of the spec, written by the LLM when implementation reveals a gap. Example: "Add to Scenarios: Scenario 5 — concurrent requests from same IP under threshold." The human approves the delta, edits the spec, then implementation continues.

## What if I want autonomous loops (Ralph-style)?

This template doesn't support them. Specs and AC are the loop's stopping criterion in autonomous systems; the rest of the workflow is identical. If you want autonomy, run your loop against the spec and treat AC as the completion test.

## Why aren't there CI workflows in `.github/workflows/`?

Stack-specific. You add what fits your test/lint setup. The `.github/` directory ships only issue forms and a PR template.

## Why no Anti-scope section in the feature template?

Scope already has "out of scope." Adding Anti-scope was redundant. If you find common LLM scope creep in your project, add it project-specifically.

## Should I commit the design doc in `docs/superpowers/specs/`?

It was committed during template development to show how the design was reached. You can keep it for reference or delete it — your call.

## Where do I push back on this workflow?

If a rule causes more friction than it prevents, change it. The constitution is the contract; everything else is convention. Edit your fork.
