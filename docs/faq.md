# FAQ

## Getting started

<details>
<summary><strong>What does <code>L1 / L2 / L3</code> complexity mean?</strong></summary>

A label on each feature spec to set expectations:

- **L1** — trivial change (small fix, one file). No ADR.
- **L2** — normal feature (several files, no architectural impact). No ADR.
- **L3** — architecturally significant (changes module boundaries, swaps a library, affects multiple domains). Write an ADR alongside the feature spec.

The workflow is the same for all three; L3 just adds the ADR.
</details>

<details>
<summary><strong>What's a "constitution"?</strong></summary>

`spec/00-constitution.md` — the rules of the project itself: stack, test command, branching, commit format, when ADRs are required. Stable, rarely changes. The AI reads it for context; you read it when something's ambiguous.
</details>

<details>
<summary><strong>What's a "spec delta"?</strong></summary>

A concrete proposed change to one or more spec sections, written by the LLM when implementation reveals a gap. Example: *"Add Scenario 5 — concurrent requests from same IP under threshold."* Human approves the delta, edits the spec, implementation continues.
</details>

## Workflow choices

<details>
<summary><strong>Why only 3 statuses?</strong></summary>

`draft → approved → done` gates what matters: no code before approval, no premature "done." Adding `in_review`, `in_progress`, etc. duplicates state that's already obvious from the Progress log or the `load:` list.
</details>

<details>
<summary><strong>Why no <code>phase</code> field?</strong></summary>

Status carries it. `draft` = refining the spec. `approved` = implementing or verifying. A phase field is a second source of truth that drifts.
</details>

<details>
<summary><strong>Why is STATE.md just a pointer?</strong></summary>

Per-feature history lives in the feature spec's `## Progress` log. State attached to the spec it describes. STATE.md answers one question: "which feature is active right now?"
</details>

<details>
<summary><strong>Why no Plan step?</strong></summary>

The original SDD pipeline had `Spec → Plan → Tasks → Implement → Verify`. We dropped Plan because for L1/L2 it duplicates Tasks. For L3, the ADR is the plan.
</details>

<details>
<summary><strong>Why is the Progress log inside the feature spec, not separate?</strong></summary>

1. Loading the spec loads the history — single file, full context.
2. Append-only logs in one file are easier to keep correct than separate state files that have to stay in sync.
</details>

## Provider files

<details>
<summary><strong>Why two provider files instead of one?</strong></summary>

Claude Code auto-loads `CLAUDE.md`. Codex auto-loads `AGENTS.md`. The auto-load convention is fixed by the tool, so we provide both. SDD rules stay in sync; tool hints diverge per runtime.
</details>

<details>
<summary><strong>What if I use both Claude Code and Codex?</strong></summary>

Keep both files. They share SDD rules. Each tool reads only its own bootstrap.
</details>

<details>
<summary><strong>How do I add a new AI (Cursor, Aider, …)?</strong></summary>

Add the top-level file the tool expects (Cursor uses `.cursor/rules/*.mdc`, Aider uses `.aider.conf.yml`, etc. — check the tool's docs). Mirror the SDD rules from `CLAUDE.md`. The `spec/` directory is provider-agnostic.
</details>

## Edge cases

<details>
<summary><strong>What about hotfixes for a <code>done</code> feature?</strong></summary>

Two cases:

- Spec was correct, implementation had a bug → new feature spec for the fix (e.g., `F042-fix-login-rate-limit-edge.md`).
- Spec was wrong → spec delta on the original feature, re-approve, new Progress entries.
</details>

<details>
<summary><strong>Can I have multiple active features in parallel?</strong></summary>

`STATE.md` tracks one. Pick the priority feature. If you truly need parallel work, use multiple branches — each with its own STATE checkout. Easier to swap `active_feature` than to coordinate two.
</details>

<details>
<summary><strong>What about pure refactors with no behavior change?</strong></summary>

No feature spec needed. PR description explains the rationale. If the refactor is architectural (module boundaries, swapping a library), write an ADR. Otherwise just a regular PR.
</details>

<details>
<summary><strong>What about autonomous loops (Ralph-style)?</strong></summary>

This template doesn't drive them directly. Specs and AC are the loop's stopping criterion in autonomous systems; the rest is identical. Run your loop against the spec, treat AC as the completion test.
</details>

## What's not in the template

<details>
<summary><strong>Why no CI workflows in <code>.github/workflows/</code>?</strong></summary>

Stack-specific. Add what fits your test/lint setup. The `.github/` directory ships only a bug template and a PR template.
</details>

<details>
<summary><strong>Why no Anti-scope section in the feature template?</strong></summary>

Scope already has "out of scope." Adding Anti-scope was redundant. If you find common LLM scope creep in your project, add it project-specifically.
</details>

<details>
<summary><strong>Where do I push back on this workflow?</strong></summary>

If a rule causes more friction than it prevents, change it. The constitution is the contract; everything else is convention. Edit your fork.
</details>
