---
name: atomic-task
description: Execute one backlog task with Codex CLI using small, reviewable changes and repository-native validation.
---

# Atomic Task

Use this skill when implementing a task from `BACKLOG.md` or a similarly scoped user request.

## Execution Rules

- Keep the task to one functional responsibility.
- Prefer existing project patterns over new abstractions.
- Modify only the files required by the task and nearby tests/docs.
- Preserve unrelated local changes.
- Do not run destructive commands unless the user explicitly requested them.
- Treat generated artifacts, migrations, lockfiles, and snapshots as high-impact files that require extra justification.

## Workflow

1. Ensure context has been loaded. For backlog work, use `session-start`; for direct user requests, use `context-load`.
2. Restate the implementation boundary in one or two sentences.
3. Inspect the target code and tests.
4. Make the smallest coherent patch.
5. Run focused validation first, then broader validation if risk justifies it.
6. Review `git diff` for accidental churn, secrets, debug output, and unrelated edits.
7. Report what changed, what was validated, and any residual risk.

## Validation Heuristics

- Code change: run unit tests for the touched module when available.
- Shared logic: run the broader test or type-check command.
- Docs/template change: validate links, headings, file references, and consistency with related templates.
- Security/privacy change: run the configured SAST or privacy checklist from project context.

## Handoff

If the task should be reviewed by Gemini or Claude, produce a short handoff:

```text
Context:
Change:
Files:
Validation:
Open questions:
```
