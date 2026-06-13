# FAQ

<details>
<summary><strong>Why only 3 statuses?</strong></summary>

`draft → approved → done` gates what matters: no code before approval, no premature "done." Adding `in_review`, `in_progress`, etc. duplicates state already obvious from the Progress log or `load:` list.
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
<summary><strong>Why two provider files instead of one?</strong></summary>

Claude Code auto-loads `CLAUDE.md`. Codex auto-loads `AGENTS.md`. The auto-load convention is fixed by the tool. SDD rules stay in sync; tool hints diverge per runtime.
</details>

<details>
<summary><strong>What if I use both Claude Code and Codex?</strong></summary>

Keep both files. They share SDD rules. Each tool reads only its own bootstrap.
</details>

<details>
<summary><strong>How do I add a new agent (Cursor, Aider, …)?</strong></summary>

Add a new top-level file (`.cursorrules`, `AIDER.md`) following that tool's convention. Mirror SDD rules from `CLAUDE.md`. `spec/` is provider-agnostic.
</details>

<details>
<summary><strong>Why no Plan step?</strong></summary>

The original SDD pipeline had `Spec → Plan → Tasks → Implement → Verify`. We dropped Plan because for L1/L2 it duplicates Tasks. For L3 (architecture-impact), the ADR is the plan.
</details>

<details>
<summary><strong>Why is the Progress log inside the feature spec, not separate?</strong></summary>

1. Loading the spec loads the history. Single file, full context.
2. Append-only logs are easier to write correctly than separate state files that have to stay in sync.
</details>

<details>
<summary><strong>What's a "spec delta"?</strong></summary>

A concrete proposed change to one or more spec sections, written by the LLM when implementation reveals a gap. Example: *"Add Scenario 5 — concurrent requests from same IP under threshold."* Human approves the delta, edits the spec, implementation continues.
</details>

<details>
<summary><strong>What about autonomous loops (Ralph-style)?</strong></summary>

This template doesn't support them directly. Specs and AC are the loop's stopping criterion in autonomous systems; the rest is identical. Run your loop against the spec, treat AC as the completion test.
</details>

<details>
<summary><strong>Why no CI workflows in <code>.github/workflows/</code>?</strong></summary>

Stack-specific. Add what fits your test/lint setup. The `.github/` directory ships only issue forms and a PR template.
</details>

<details>
<summary><strong>Why no Anti-scope section in the feature template?</strong></summary>

Scope already has "out of scope." Adding Anti-scope was redundant. If you find common LLM scope creep in your project, add it project-specifically.
</details>

<details>
<summary><strong>Should I commit the design doc in <code>docs/superpowers/specs/</code>?</strong></summary>

It was committed during template development to show how the design was reached. Keep it for reference or delete it — your call.
</details>

<details>
<summary><strong>Where do I push back on this workflow?</strong></summary>

If a rule causes more friction than it prevents, change it. The constitution is the contract; everything else is convention. Edit your fork.
</details>
