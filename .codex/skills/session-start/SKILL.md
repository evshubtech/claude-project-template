---
name: session-start
description: Start a Codex implementation session by loading shared memory, selecting the next ready backlog task, and confirming scope with the user before execution.
---

# Session Start

Use this skill at the beginning of a product-development session when Codex will execute a task from `BACKLOG.md`.

This is the Codex equivalent of the Claude `/session-start` workflow. It composes the smaller `context-load` skill but adds task selection, readiness checks, and explicit user confirmation.

## Goal

Start work with enough context to execute one atomic task safely, without loading unnecessary history or guessing the user's intended task.

## Required Inputs

- `AGENTS.md`
- `CODEX.md` or `TEMPLATE_CODEX.md` when bootstrapping
- `CONTEXT.md`
- `BACKLOG.md`
- `SESSION_LOG.md` when present
- `.codex/skills/context-load/SKILL.md`

## Workflow

### 1. Load Context

Run the `context-load` workflow:

1. Read `AGENTS.md`.
2. Read `CODEX.md`. If missing in a template-maintenance session, read `TEMPLATE_CODEX.md`.
3. Read `CONTEXT.md`.
4. Read `BACKLOG.md`.
5. Read only the latest relevant block from `SESSION_LOG.md` if present.

If `CONTEXT.md` or `BACKLOG.md` is missing in a product project, stop and report the missing file. If this is the template repository itself, continue only if the user's task is explicitly template maintenance.

### 2. Summarize Project State

Display:

- Current sprint number and objective.
- Last context update.
- Active blockers, or "None".
- Last session summary, including failures and next step when available.

Keep this summary short. Do not paste full files.

### 3. Select Candidate Task

Find the first task in `BACKLOG.md` with:

```text
**Status:** PRONTO
```

Treat the first `PRONTO` task as highest priority unless the user gave a specific task ID.

If no `PRONTO` task exists, stop and report:

```text
Nenhuma tarefa PRONTO no backlog. Atualize o BACKLOG.md com o Gemini antes de continuar.
```

### 4. Check Readiness

For the selected task, report:

- Task ID.
- Parent epic.
- Short summary.
- LGPD classification.
- Dependencies.
- Whether all DoR checklist items are complete.

If DoR is incomplete, list the pending items and ask whether the user wants to continue anyway. Do not implement until the user confirms.

If the task is `BLOQUEADO`, stop and explain the blocker.

### 5. Confirm With User

Ask:

```text
Confirmamos trabalhar em TASK-XXX - [summary]? (sim/nao)
```

If the user says yes:

- Display the task's `Prompt para o Executor`.
- State the likely edit boundary.
- State the validation commands Codex will try.
- Continue with the `atomic-task` skill only after confirmation.

If the user says no:

- Ask for the desired task ID.
- Repeat task summary, readiness check, and confirmation.

## Output Contract

Before implementation starts, Codex must have produced:

- Selected task ID.
- Confirmed scope.
- LGPD classification.
- Expected files or directories to inspect.
- Validation plan.

## Best Practices

- Prefer confirmation over guessing when multiple tasks are ready.
- Do not mark the task as `EM PROGRESSO` during session start unless the user explicitly asks. Status updates belong to closeout or task execution policy.
- Do not load old session logs unless the latest block points to older context.
- Do not execute code changes inside this skill. This skill prepares execution; `atomic-task` executes it.
