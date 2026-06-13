# Migrating an existing project to SDD

> You already have a codebase. You want spec-driven development going forward without rewriting history. Here's how.

## Pick a path

| Path | When                                              | Cost              |
| ---- | ------------------------------------------------- | ----------------- |
| A    | Default. Spec only new work.                      | ~10 min one-time  |
| B    | Hot modules you keep changing.                    | ~30 min / module  |
| C    | Full rewrite-with-specs.                          | Don't.            |

Start with A. Add B only when a change makes you ask "what does this even do?"

---

## Path A — Lazy retrofit (recommended)

Existing code stays as-is. Specs apply only to features you write from today onward.

### 1. Copy the template files into your repo

From this template, copy:

```
CLAUDE.md                       # Claude Code bootstrap
AGENTS.md                       # Codex bootstrap (delete one you don't use)
spec/00-constitution.md
spec/01-rules-llm.md
spec/STATE.md
spec/README.md
spec/features/F000-template.md
spec/adr/ADR-000-template.md
.github/PULL_REQUEST_TEMPLATE.md
.github/ISSUE_TEMPLATE/bug.md
docs/quickstart.md
docs/walkthrough.md
docs/faq.md
docs/superpowers.md
```

Quick way:

```bash
# from your existing project root
git clone --depth=1 https://github.com/mgummich/sdd-workflow.git /tmp/sdd
cp -r /tmp/sdd/spec ./
cp /tmp/sdd/CLAUDE.md ./       # or AGENTS.md
mkdir -p docs .github/ISSUE_TEMPLATE
cp /tmp/sdd/docs/*.md ./docs/
cp /tmp/sdd/.github/PULL_REQUEST_TEMPLATE.md ./.github/
cp /tmp/sdd/.github/ISSUE_TEMPLATE/bug.md ./.github/ISSUE_TEMPLATE/
rm -rf /tmp/sdd
```

### 2. Merge clashes, don't overwrite

| File you may already have       | Action                                                     |
| ------------------------------- | ---------------------------------------------------------- |
| `CLAUDE.md` / `AGENTS.md`       | Prepend SDD bootstrap to top of yours. Keep your rules.    |
| `.github/PULL_REQUEST_TEMPLATE` | Add "Spec Reference" + AC checklist sections. Keep rest.   |
| `README.md`                     | Add link to `spec/README.md`. Otherwise untouched.         |
| `docs/`                         | Add SDD docs alongside yours.                              |

Never blast over existing project conventions. SDD adopts the project, not the other way around.

### 3. Fill the constitution

Edit `spec/00-constitution.md`:

- Stack (language, framework versions)
- Test command (`pnpm test`, `pytest`, `cargo test`, …)
- Lint command
- Type-check command (if applicable)
- Build command

The constitution is what every session's LLM treats as ground truth for "how do I run things?"

### 4. Reset STATE.md

```yaml
---
active_feature: null
load: []
---
```

### 5. First feature

```bash
cp spec/features/F000-template.md spec/features/F001-<your-feature>.md
```

Follow [`docs/walkthrough.md`](walkthrough.md).

---

## Path B — Backfill critical surfaces

Use this only for code that will keep changing. Cold code stays uncovered.

### When to backfill

- Module is touched in >2 PRs per month.
- Public API the rest of the team consumes.
- Compliance-relevant code (auth, billing, data export).

### How to backfill

1. Do Path A first.
2. Pick one module. Resist scope creep — one module.
3. In the AI session: `Use the spec-miner skill to reverse-engineer spec/features/FNNN-<module>.md from src/<module>/`. (See `fullstack-dev-skills:spec-miner`.)
4. Review the generated spec. Trim ruthlessly. Keep only Intent, Contracts, Scenarios, AC that reflect *current* behavior — not aspirational behavior.
5. Set `status: done` in frontmatter. The code already exists; AC describes shipped behavior.
6. Commit the spec.

Next time you change that module:

1. Flip `status: draft`.
2. Edit the spec to reflect the change (new contract, new scenario).
3. Flip `status: approved`.
4. Implement the delta. Normal SDD loop.

### Backfill anti-patterns

- Writing aspirational specs ("here's what it *should* do"). Spec = current truth. Improvements = new feature spec.
- Backfilling every file. You will burn out. Top 3 modules max in week 1.
- Backfilling test files. Tests aren't specs; they're verification.
- Keeping the spec on a branch "until the PR merges." Specs land in `main` immediately so they're loadable.

---

## Path C — Full rewrite-with-specs

Don't. You'll never finish. If the existing code is so bad you'd rewrite it anyway, do that as a *project*, with its own spec, scoped to one module at a time, using Path B.

---

## Repo-shape gotchas

### Monorepos

Default: one `spec/` at repo root, features prefixed by package:

```
spec/features/F001-api-rate-limit.md
spec/features/F002-web-checkout-flow.md
```

Per-package `spec/` only when packages ship independently with separate release cycles. Otherwise the per-package split duplicates STATE.md and confuses cross-package features.

### Polyrepos

One template install per repo. Each repo has its own `spec/`, `STATE.md`, constitution.

### Existing `docs/` directory

SDD docs are user-facing guides. If your `docs/` is for users of your product, put SDD docs in `docs/dev/` or `.sdd/docs/` instead. Update `README.md` links.

### CI

SDD adds no required CI step. Optional hardening:

- Lint that every PR touching `src/` references a spec file in the PR body.
- Block merge if any `spec/features/F*.md` has `status: draft` and matching code changes.

Add these only after the team is using SDD without coercion. Premature enforcement breeds spec-theater.

---

## Migration checklist

Copy into a tracking issue:

```markdown
- [ ] Copied template files (Path A step 1)
- [ ] Merged CLAUDE.md / AGENTS.md without losing existing rules
- [ ] Updated PR template with Spec Reference + AC sections
- [ ] Filled spec/00-constitution.md with real commands
- [ ] STATE.md reset to active_feature: null
- [ ] First F001 spec drafted and approved
- [ ] First F001 implemented end-to-end with the new loop
- [ ] (Optional) Backfilled spec for one hot module (Path B)
- [ ] Team agreement: new work follows SDD; old code untouched
```

---

## When SDD isn't worth it

Skip the migration if:

- Project is a one-off script or throwaway prototype.
- Team is solo + short-lived + no AI in the loop.
- Codebase is in active deletion (sunsetting).

SDD pays off when: multiple contributors, AI in the loop, code lives >6 months, change pace > 1 PR/week.
