# 00 — Project Constitution

## 0. Stack

<!-- Replace these markers for your project. -->

- **Language/runtime:** `<TODO: tech-stack>`
- **Test command:** `<TODO: test-command>`
- **Lint command:** `<TODO: lint-command>`
- **Pre-commit:** `<TODO: pre-commit-command or n/a>`

## 1. Spec-Driven Development

Specifications are the **single source of truth**.

**Workflow:**

```
Spec → Approve → Implement → Verify
```

**Status:**

```
draft → approved → done
```

- `draft` — being written or reviewed
- `approved` — implementation green-lit
- `done` — AC all green, verified

**Gates:**

- No code until `status: approved`.
- If implementation reveals a spec gap: stop, propose spec delta, wait for spec update.

Artifacts per feature:

- `spec/features/Fxxx-*.md` — feature spec including AC and Progress log
- Optional: `spec/adr/ADR-xxx-*.md` — architecture decision

**Progress log compaction:** entries older than 30 days may be collapsed into a single summary; keep the last 5 verbatim.

## 2. Quality & Testing

- Every change requires automated tests.
- AC live in the feature spec and must be enforced by tests.
- CI (lint + unit tests) should complete in < 10 minutes.

## 3. Git & Commits

- Conventional Commits (feat, fix, docs, refactor, test, chore).
- Trunk-based: short-lived feature branches off `main`, merged via PR.
- No code goes directly to `main`/`trunk` without tests.
- `--no-verify` is not part of the normal workflow.

## 4. Architecture Decisions (ADRs)

ADRs only for decisions that are:

- hard to reverse,
- long-term impact,
- affecting multiple teams/domains.

Format: `spec/adr/ADR-xxx-*.md` (1–2 pages). Status: Proposed → Accepted → Deprecated/Superseded.

## 5. Documentation

- **README.md** — what the project is, how to run it locally.
- **Dev docs** (optional) — `docs/development/*.md`.
- Feature-specific rules live in the feature spec.

## 6. LLM Usage

Pass only:

- `spec/01-rules-llm.md`
- the active feature spec
- 1–2 ADRs if the feature references them (add them to `load:` in STATE.md)

Token efficiency: search/snippets over full files; structured output; per-feature Progress log instead of growing history.

## 7. Verify

`status: done` requires:

1. Every `## Acceptance Criteria` box checked.
2. Tests pass.
3. Final Progress entry: `Verified. AC all green.`
4. `status: done`.
