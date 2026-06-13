# Superpowers + SDD

> How to use the `superpowers:*` skill pack inside this Spec-Driven Development workflow without breaking the gates.

Superpowers are process skills. SDD is the process. They compose — but SDD always wins. If a skill says "go" and the spec says `draft`, the spec wins.

---

## Mental model

```
SDD phase        Superpowers role
─────────        ─────────────────
Draft spec   →   shape intent (brainstorming, common-ground)
Plan         →   turn approved spec into steps (writing-plans)
Approve      →   gate. No skill bypasses this.
Implement    →   execute steps (executing-plans, TDD, worktrees)
Debug        →   any failure (systematic-debugging)
Verify       →   confirm AC (verification-before-completion)
Review       →   request/receive review
Finish       →   merge / PR (finishing-a-development-branch)
```

---

## Invocation rule

Use the `Skill` tool. Never `Read` a skill file.

```
Skill(skill="superpowers:brainstorming")
```

Announce before acting: `Using superpowers:brainstorming to explore F001 intent`.

---

## Phase 1 — Draft (`status: draft`)

Goal: shape intent into a complete spec. **No code.**

Skills:

- `superpowers:brainstorming` — required before drafting any new feature. Surfaces user intent, edge cases, non-goals.
- `fullstack-dev-skills:common-ground` — surface hidden assumptions, get user to confirm before they harden into spec text.
- `superpowers:writing-skills` — only if drafting a new skill, not a feature.

Output: filled-out `spec/features/FNNN-*.md` with Intent, Contracts, Scenarios, AC. Status stays `draft`.

Anti-pattern: jumping to `test-driven-development` while spec is draft. Tests encode contracts. Contracts not approved → tests not allowed.

---

## Phase 2 — Plan (between draft and approved)

Goal: turn spec scenarios + AC into an ordered step list.

Skills:

- `superpowers:writing-plans` — multi-step task → written plan before code.

Output: plan in spec body or sibling file. Human reviews, then flips `status: approved`.

Skip if feature is trivial (L1, one scenario).

---

## Phase 3 — Approve gate

Human-only. No skill. Frontmatter flip:

```yaml
status: approved
```

Update `spec/STATE.md` `active_feature` + `load:`.

---

## Phase 4 — Implement (`status: approved`)

Goal: code matches spec. Spec wins every conflict.

Skills:

- `superpowers:using-git-worktrees` — isolate the feature branch before touching code.
- `superpowers:executing-plans` — drive the plan with review checkpoints.
- `superpowers:subagent-driven-development` — alternative when plan steps are independent and runnable in current session.
- `superpowers:test-driven-development` — red/green/refactor per AC item.
- `superpowers:dispatching-parallel-agents` — when 2+ truly independent tasks exist.

Spec-delta gate (from `CLAUDE.md`): if implementation reveals a spec gap, **stop**. Propose a delta. Wait for human. Do not patch silently. No skill overrides this.

Anti-pattern: `systematic-debugging` to "make the test pass" when the test itself encodes a wrong contract. Fix the spec, then the test.

---

## Phase 5 — Debug (any phase)

Skills:

- `superpowers:systematic-debugging` — required before proposing any fix for a bug, test failure, or unexpected behavior.
- `fullstack-dev-skills:debugging-wizard` — deep stack/log analysis.

Output: hypothesis → minimal repro → root cause → fix. Not "try a thing."

---

## Phase 6 — Verify (`status: approved` → `done`)

Goal: walk AC checklist. Capture evidence.

Skills:

- `superpowers:verification-before-completion` — gates "done."
- `verify` (built-in) — run the app, observe behavior, not just tests.

Output: AC boxes checked in spec. Progress entry with green test output reference.

---

## Phase 7 — Review

Skills:

- `superpowers:requesting-code-review` — when implementation complete, before merge.
- `superpowers:receiving-code-review` — handle feedback.
- `code-review:code-review` or `/code-review` — local diff review.

PR body must link the spec and tick every AC box (see `walkthrough.md`).

---

## Phase 8 — Finish

Skills:

- `superpowers:finishing-a-development-branch` — pick merge / PR / cleanup path.
- `ship:ship` — full PR-to-prod loop if applicable.

Then:

1. Flip spec `status: done`.
2. Reset `STATE.md` (`active_feature: null`, `load: []`).
3. Append final Progress entry.

---

## Session-start checklist

Every session, before any skill:

1. Read `spec/STATE.md`.
2. If `active_feature` set: read every `load:` path.
3. Pin active spec in working memory.
4. Then invoke skills.

Skip this and skills will give locally-correct but spec-divergent output.

---

## Gate cheatsheet

| Gate          | Trigger                            | Action                                      |
| ------------- | ---------------------------------- | ------------------------------------------- |
| Status gate   | `status: draft`                    | Refine spec only. No code, no tests.        |
| Spec-delta    | Code needs behavior not in spec    | Stop. Propose delta. Wait for human.        |
| Verification  | "I think it's done"                | Run `verification-before-completion`.       |
| Approve       | Spec → implementation              | Human flips frontmatter. Skill can't.       |
| Done          | All AC ticked + verified           | Human flips `status: done`.                 |

---

## Anti-patterns

- Invoking `test-driven-development` while spec is `draft`.
- Letting `systematic-debugging` rewrite contracts to make tests pass.
- Skipping `brainstorming` because "the user already explained it."
- Using `Read` on a skill file instead of `Skill` tool.
- Running `finishing-a-development-branch` before AC checklist complete.
- Adding skills to `STATE.md` `load:` — skills aren't specs.

---

## Minimal flow (copy-paste)

```
1. Read spec/STATE.md
2. Skill: superpowers:brainstorming         # draft only
3. Write spec → human approves
4. Skill: superpowers:writing-plans         # if non-trivial
5. Skill: superpowers:using-git-worktrees
6. Skill: superpowers:executing-plans
   ├─ Skill: superpowers:test-driven-development
   └─ Skill: superpowers:systematic-debugging   # on failure
7. Skill: superpowers:verification-before-completion
8. Skill: superpowers:requesting-code-review
9. Skill: superpowers:finishing-a-development-branch
10. Flip status: done. Reset STATE.md. Append Progress.
```
