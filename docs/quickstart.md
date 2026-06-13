# Quickstart

5 minutes from cloning to first approved spec.

## 1. Create your repo

Click **Use this template** on GitHub → new repo. Clone it.

## 2. Personalize the constitution

Open `spec/00-constitution.md`. Replace the `<TODO>` markers in §0:

```markdown
- Language/runtime: TypeScript / Node 22
- Test command: pnpm test
- Lint command: pnpm lint
```

These three lines tell the LLM how your project actually builds and tests.

## 3. Pick your provider

Keep one, delete the other:

- Using Claude Code → keep `CLAUDE.md`, delete `AGENTS.md`
- Using Codex → keep `AGENTS.md`, delete `CLAUDE.md`
- Using both → keep both

## 4. Open your agent

Claude Code auto-loads `CLAUDE.md`. Codex auto-loads `AGENTS.md`. The agent will read `spec/STATE.md`, see `active_feature: null`, and ask which feature to start. That's the signal you're set up correctly.

## 5. Draft your first feature spec

Copy the template:

```bash
cp spec/features/F000-template.md spec/features/F001-my-feature.md
```

Edit the frontmatter:

```yaml
id: F001
status: draft
complexity: L1
architectureImpact: false
```

Fill in `Intent`, `Scope`, `Business Rules`, `Contracts`, `Scenarios`, `Acceptance Criteria`. Ask the agent to help — that's what `draft` status is for.

## 6. Approve and start implementing

When the spec is ready, set `status: approved` and update `spec/STATE.md`:

```yaml
---
active_feature: F001
load:
  - spec/01-rules-llm.md
  - spec/features/F001-my-feature.md
---
```

Tell the agent to implement. It will append a `## Progress` entry at the end of the session.

## 7. Verify

Walk the `## Acceptance Criteria` checklist, run tests, append `Verified. AC all green.` to Progress, flip `status: done`.

That's the loop. Next feature: copy the template again, increment to `F002`, repeat.

---

See [`walkthrough.md`](walkthrough.md) for a concrete example. See [`faq.md`](faq.md) for the why behind these choices.
