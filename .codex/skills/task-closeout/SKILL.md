---
name: task-closeout
description: Close a Codex task with validation evidence, session memory updates, and a clean handoff to shared agent workflows.
---

# Task Closeout

Use this skill as the reusable closeout component when Codex has completed an implementation or template-maintenance task.

For backlog-driven product work, prefer `task-done`, which composes this skill and adds explicit Gate 0, scoring, SAST, ADR, diff, memory, and commit-confirmation steps.

## Closeout Steps

1. Run or explain validation.
2. Review `git diff` and `git status --short`.
3. Update `SESSION_LOG.md` only in instantiated projects. Do not create it in the template repository unless explicitly requested.
4. If working from `BACKLOG.md`, update only the task status and checklist items that were actually satisfied.
5. If the work produced an architectural decision, propose an ADR or ask whether the user wants Gemini `/adr` to own it.
6. Provide a concise final summary with changed files and validation result.

## Shared Memory Rules

- `CONTEXT.md`: update for project state, critical decisions, active blockers, and next priority.
- `BACKLOG.md`: update task status, DoR/DoD, and blockers.
- `SESSION_LOG.md`: append-only; never rewrite older session blocks.
- `PLANO_DIRETOR.md`: update only for template-level operating-model decisions.

## Commit Boundary

Codex may prepare a commit message and summarize the diff, but should not commit unless the user explicitly asks for a commit.

Suggested commit message format:

```text
type(scope): short imperative summary
```
