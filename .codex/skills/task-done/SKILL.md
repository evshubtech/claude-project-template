---
name: task-done
description: Complete a Codex task after implementation and Gemini Gate 0 review by running quality scoring, security checks, memory updates, and explicit commit confirmation.
---

# Task Done

Use this skill when Codex has completed implementation and the user is ready to close the task.

This is the Codex equivalent of the Claude `/task-done` workflow. It composes `review-diff` and `task-closeout`, but makes the gate sequence explicit.

## Usage

```text
task-done TASK-XXX
```

If no task ID is provided, infer the task only when there is exactly one active or recently implemented task in context. Otherwise ask for the task ID.

## Preconditions

- Implementation is complete.
- Relevant tests or validation have been run, or there is a clear reason they cannot run.
- Gemini Gate 0 was completed for critical tasks, or the user explicitly says Gate 0 is not required.
- The worktree may contain user changes; Codex must preserve unrelated changes.

## Gate 0 - Gemini Review Confirmation

Ask the user whether Gemini `/review` approved the implementation:

```text
Gate 0 Gemini aprovado para TASK-XXX? (sim/nao/nao se aplica)
```

- If `sim`: continue.
- If `nao se aplica`: continue only if the task is non-critical and the reason is clear.
- If `nao`: stop. Ask the user to provide the Gemini correction brief or run Gemini review first.

Critical tasks include auth, authorization, data migrations, billing, multi-tenancy, privacy-sensitive flows, infrastructure changes, and security controls.

## Gate 1 - Codex Quality Scoring

Review the implementation and score it from 0 to 10:

| Criterion | Weight | Scoring Guidance |
|---|---:|---|
| Acceptance criteria | 4 | All task ACs satisfied, including edge cases |
| Security / privacy | 3 | LGPD classification respected, no secrets/PII leaks, no obvious injection/auth risk |
| Tests / validation | 2 | Focused validation run, meaningful coverage, failure path considered |
| Maintainability | 1 | Local patterns preserved, no unnecessary abstraction or unrelated churn |

Compute a weighted total.

If score is below 7:

- Stop.
- Produce a restart brief in English:

```text
Context:
Problem found:
Required fix:
Acceptance criteria to revalidate:
Validation to run:
```

Do not continue to Gate 2.

## Gate 2 - Security and Static Analysis

Run the security command configured in `CODEX.md`.

If `CODEX.md` does not define a security command, inspect `CLAUDE.md` for `SECURITY_TOOL` or equivalent. If no tool is configured, report that Gate 2 is skipped because no security command is configured.

Block on:

- HIGH or CRITICAL vulnerabilities.
- Secret exposure.
- PII logging introduced by the task.
- Authorization or tenant-isolation regression.

If blocked, report the finding and stop before memory updates or commit.

## Gate 2.5 - ADR Check

Ask:

```text
Esta task gerou uma decisão arquitetural significativa? (sim/nao)
```

If yes:

- Prefer asking Gemini `/adr` to generate the ADR when the decision is strategic.
- Codex may draft a local ADR only when the user asks and `ADR_PATH` is configured.
- Do not hide architectural decisions inside session logs only.

If no, continue.

## Gate 3 - Diff Review and Commit Confirmation

Run the `review-diff` workflow:

1. Inspect `git status --short`.
2. Inspect `git diff --stat`.
3. Inspect `git diff`.
4. Identify which changed files belong to this task and which are unrelated.

Prepare a Conventional Commit message:

```text
type(scope): short imperative summary
```

Display:

- Files to include.
- Files to leave untouched.
- Commit message.
- Validation evidence.

Ask:

```text
Confirma atualizar memória compartilhada e preparar commit para TASK-XXX? (sim/nao)
```

If no, stop and explain what remains.

## Memory Updates

If confirmed:

1. Append a `[CODEX]` block to `SESSION_LOG.md` in Portuguese when the file exists.
2. Update the task in `BACKLOG.md`:
   - `EM PROGRESSO` or `PRONTO` to `CONCLUIDO`
   - Check only DoD items that were actually satisfied.
3. Update `CONTEXT.md` only if project state, blockers, current priority, or recent decisions changed.

Do not create `SESSION_LOG.md` in the template repository unless explicitly requested.

## Commit Policy

Codex must not commit unless the user explicitly confirms the commit after seeing the diff and commit message.

If the user confirms commit:

1. Stage only files that belong to the task plus approved memory files.
2. Run `git diff --cached`.
3. Ask one final confirmation:

```text
Confirma o commit com este diff staged? (sim/nao)
```

4. Commit only after `sim`.

If the user does not confirm, leave changes unstaged or staged as-is and report the next action.

## Final Output

Report:

- Gate 0 status.
- Gate 1 score.
- Gate 2 result.
- ADR result.
- Memory files updated.
- Commit status.
- Residual risks or skipped checks.
