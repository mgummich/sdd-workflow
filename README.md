# SDD Workflow Template

Minimal spec-driven development template for AI-assisted projects. Optimized for [Claude Code](https://docs.claude.com/en/docs/claude-code) and [Codex](https://openai.com/codex).

## What it gives you

- A short, stable workflow: `Spec → Approve → Implement → Verify`
- Per-feature specs as the source of truth (Markdown, in-repo)
- A token-efficient context contract for LLMs
- Provider bootstraps (`CLAUDE.md`, `AGENTS.md`) the agents auto-load

## Quick start

1. Click **Use this template** → create a new repo.
2. Edit `spec/00-constitution.md` — replace `<TODO>` markers (stack, test command, lint command).
3. Delete the provider file you don't use (`CLAUDE.md` or `AGENTS.md`).
4. Open your agent. It reads `CLAUDE.md` / `AGENTS.md`, sees `active_feature: null`, and asks which feature to start.
5. Copy `spec/features/F000-template.md` → `spec/features/F001-<your-feature>.md`, draft, approve, implement.

Full walkthrough: [`docs/walkthrough.md`](docs/walkthrough.md).

## Layout

```
.
├── CLAUDE.md          # Claude Code bootstrap
├── AGENTS.md          # Codex bootstrap
├── docs/              # User-facing guides
│   ├── quickstart.md
│   ├── walkthrough.md
│   └── faq.md
├── spec/
│   ├── 00-constitution.md   # Project principles, workflow, gates
│   ├── 01-rules-llm.md      # Provider-agnostic LLM rules
│   ├── STATE.md             # Pointer to the active feature
│   ├── features/F000-template.md
│   └── adr/ADR-000-template.md
└── .github/           # Issue forms + PR template
```

## Workflow

```
Spec → Approve → Implement → Verify
```

Status: `draft → approved → done`.

Two gates:

- **Status gate** — no code until `status: approved`
- **Spec-delta gate** — if implementation reveals a spec gap, stop and propose a spec delta

That's the whole thing.

## Docs

- [Quickstart](docs/quickstart.md) — 5-minute setup
- [Walkthrough](docs/walkthrough.md) — one feature from spec to done
- [FAQ](docs/faq.md) — design choices, common questions
- [`spec/README.md`](spec/README.md) — in-repo workflow reference

## License

MIT.
