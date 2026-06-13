# SDD Workflow Template

A minimal, token-optimized **Spec-Driven Development** template for consistent AI-assisted projects.

## Use this template

Click **"Use this template"** → "Create a new repository" on GitHub.

## What's inside

```
spec/
├── 00-constitution.md      # Project-wide principles (edit for your stack)
├── 01-rules-llm.md         # Token-optimized rules for LLMs (stable)
├── STATE.md                # Context compression between work waves
├── features/
│   └── F000-template.md    # Feature spec template
└── adr/
    └── ADR-000-template.md # Architecture decision template
```

## Workflow

```
Spec → Plan → Tasks → Implement → Verify
```

1. Open a **Feature Spec** issue (from issue templates)
2. Copy `spec/features/F000-template.md` → `spec/features/F001-<name>.md`
3. Fill in Intent, Scope, Business Rules, Contracts, Scenarios, AC
4. Implement code + tests against the spec
5. PR must link the spec; acceptance criteria must be green

See [`spec/README.md`](spec/README.md) for full usage guide.

## LLM context

Minimal context per task:

```
Load:
- spec/01-rules-llm.md
- spec/features/F001-<feature-name>.md
```

Never load the full repo. See `spec/01-rules-llm.md` for token optimization rules.
