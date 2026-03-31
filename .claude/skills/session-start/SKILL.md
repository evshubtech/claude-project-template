---
name: session-start
description: Loads full project context and identifies the next ready task. Invoke with /session-start at the beginning of every work session.
---

# /session-start

## 1. Load project state

Read `CONTEXT.md` in the repository root.

If not found, stop and report:
> "CONTEXT.md not found. Create it from CONTEXT.md.template before starting the session."

Display:
- Current sprint number and objective
- Last updated date
- Active blockers (count + short list, or "None")

## 2. Load last session context

Read `SESSION_LOG.md` in the repository root.

If not found, note: "No SESSION_LOG.md found — this appears to be the first session."

If found, locate the **last block** (`## Sessão YYYY-MM-DD`) and display:
- Session date
- Completed tasks (IDs + short descriptions)
- Failures recorded (summary or "None")
- Decisions made (summary or "None")
- Recorded next step (TASK-XXX — description)

## 3. Identify the next task

Read `BACKLOG.md` in the repository root.

If not found, stop and report:
> "BACKLOG.md not found. Create it from BACKLOG.md.template before starting the session."

Locate the **first item with status `PRONTO`** (highest priority = top of list).

If no `PRONTO` item exists, stop and report:
> "Nenhuma tarefa PRONTO no backlog.
> Atualize o BACKLOG.md com o Gemini antes de continuar."

Do not execute any further actions. End the command here.

Display the task summary (only reached if a PRONTO task was found):
- Task ID and short description
- Parent epic
- LGPD classification
- DoR complete: Yes / No (if No, list the pending checklist items)

## 4. Confirm

Ask the user:

> "Confirm we will work on **[TASK-XXX] — [description]**? (yes / no)"

- **Yes:** display the full "Prompt para o Claude Code" field from BACKLOG.md for the selected task.
- **No:** ask "Which task do you want to work on? Provide the ID (e.g. TASK-XXX).", then repeat the summary and confirmation for that task.
