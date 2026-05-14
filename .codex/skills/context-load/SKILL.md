---
name: context-load
description: Load the minimum useful project context before Codex starts implementation, review, or refactoring work.
---

# Context Load

Use this skill as the reusable context-loading component for Codex work.

For backlog-driven product work, prefer starting with `session-start`, which composes this skill and adds task selection plus user confirmation.

## Goal

Build enough context to execute safely without overloading the prompt window.

## Loading Order

1. Read `AGENTS.md` at the repository root.
2. Read `CODEX.md` if present. If missing, read `TEMPLATE_CODEX.md` only when bootstrapping a project.
3. Read `CONTEXT.md` for current project state. If missing, report that the project has not been instantiated from `CONTEXT.md.template`.
4. Read `BACKLOG.md` when working from an atomic task. If missing, ask whether this is a template-maintenance session or a product-execution session.
5. Read the latest block in `SESSION_LOG.md` when present. Do not load the full file unless the latest block references older decisions.
6. Inspect the local files directly related to the requested change.

## Context Budget

Prefer targeted reads:
- Use `rg --files`, `rg`, and focused `sed` ranges before broad file dumps.
- Load architecture or policy skills only when the task touches their domain.
- Summarize large documents into the active task context instead of rereading them repeatedly.

## Stop Conditions

Stop and ask the user before implementation if:
- The next task is blocked in `BACKLOG.md`.
- The task lacks acceptance criteria or a clear target file/module.
- Required commands are unknown and cannot be inferred from project files.
- A proposed change conflicts with an explicit `Do Not Touch` rule.

## Output

Before editing, state:
- The task being executed.
- The files or directories likely to change.
- The validation commands you will try.
