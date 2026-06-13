# 00 – Project Constitution

This document defines the core rules for this project.
It should be short, stable, and rarely changed.

## 1. Spec-Driven Development

- Specifications are the **single source of truth** for features.
- Per-feature workflow: **Spec → Plan → Tasks → Implement → Verify**.[^sdd]
- Implementation must follow the spec; if reality diverges, fix the spec first.

Artifacts per feature:
- `spec/features/Fxxx-*.md` – feature spec (including testing view).
- Optional: `spec/adr/ADR-xxx-*.md` – architecture decisions.

[^sdd]: Inspired by GitHub's spec-kit and spec-driven development workflows.

## 2. Quality & Testing

- Every change requires automated tests.
- Acceptance criteria live in the feature spec and must be enforced by tests.
- Fast feedback: CI (lint + unit tests) should usually complete in < 10 minutes.

## 3. Git & Commits

- Commit messages follow **Conventional Commits** (feat, fix, docs, refactor, test, chore).
- No code goes directly to `main`/`trunk` without tests.
- `--no-verify` is not part of the normal workflow.

## 4. Architecture Decisions (ADRs)

- ADRs are only required for **architecturally significant** decisions:
  - hard to reverse,
  - long-term impact,
  - affecting multiple teams/domains.
- Format: short Markdown (1–2 pages) under `spec/adr/ADR-xxx-*.md`.
- Status: Proposed → Accepted → Deprecated/Superseded.

## 5. Documentation

- **README.md**: what the project is, how to run it locally.
- **Dev docs** (optional): `docs/development/*.md` for local setup, pipelines, conventions.
- Feature-specific rules stay inside the feature specs.

## 6. LLM Usage

- When working with LLMs, prefer passing only:
  - `spec/01-rules-llm.md` (this project's rules for LLMs),
  - the relevant feature spec (`spec/features/Fxxx-*.md`),
  - optionally 1–2 architecture/ADR files if needed.
- Specs are written to be LLM-friendly: clear structure, short sections, concrete examples.
- **Token optimization** is a priority: use search/outlines before loading full files, prefer structured output, compress context after each wave (STATE.md).
