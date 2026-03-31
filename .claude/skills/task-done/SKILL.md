---
name: task-done
description: Runs the three-gate completion flow for a task — quality scoring, SAST, then commit. Invoke with /task-done when you believe a task is fully implemented.
---

# /task-done

Usage: `/task-done TASK-XXX`

## Step 0 — Mark task as in progress

Read `BACKLOG.md` and locate the task matching the provided ID.

If not found, stop and report:
> "Task [TASK-XXX] not found in BACKLOG.md. Check the ID and try again."

If the task status is not `PRONTO`, stop and report:
> "Task [TASK-XXX] has status [current status]. Only PRONTO tasks can be started via /task-done."

Update the task status in `BACKLOG.md`:

```
**Status:** PRONTO  →  **Status:** EM PROGRESSO
```

Confirm to the user:
> "Task [TASK-XXX] marked as EM PROGRESSO. Running quality gates…"

---

Three sequential gates. Each gate can block progress.

---

## Gate 1 — Quality Scoring (self-evaluation)

Score the work just completed on a 0–10 scale using weighted criteria:

| Criterion                  | Weight | Your Score (0–10) | Weighted |
|----------------------------|--------|-------------------|----------|
| Test coverage              | 3      |                   |          |
| Security / privacy (LGPD)  | 4      |                   |          |
| Style adherence / DRY      | 3      |                   |          |
| **Total**                  | **10** |                   | **/10**  |

**Scoring rules:**
- Test coverage: 0 = no tests, 5 = happy path only, 10 = happy + error paths + edge cases
- Security / privacy: 0 = PII exposed or injection risk present, 5 = no obvious issues, 10 = LGPD classification respected, no sensitive data in logs, inputs validated
- Style / DRY: 0 = duplicated logic or inconsistent patterns, 5 = acceptable, 10 = follows existing conventions, no unnecessary abstraction

**If total score < 7 — BLOCK:**
> Generate a restart brief with:
> - Which criterion failed and why
> - Specific actions needed to raise the score above 7
> - Stop here. Do not proceed to Gate 2.

---

## Gate 2 — SAST (Static Analysis)

Run the security analysis tool for the project's stack.
Configure the correct tool in the project's CLAUDE.md.

Examples by stack:
- Python → `bandit -r ./src`
- Node/TypeScript → `npm audit`
- TypeScript (type check) → `tsc --noEmit`
- Go → `gosec ./...`
- Ruby → `brakeman`

Block on: HIGH or CRITICAL vulnerabilities.
Skip if: no tool configured in CLAUDE.md.

**If a blocking finding is detected — BLOCK:**
> Report the finding (tool, severity, location).
> Stop here. Do not proceed to Gate 3.

---

## Gate 3 — Commit

All gates passed. Execute in order:

### 3a. Generate commit message

Prepare the commit message following Conventional Commits format:

```
type(scope): short description

# Types: feat | fix | chore | docs | refactor | test | perf
# Scope: module or file affected (e.g. auth, backlog, session)
# Example: feat(auth): add tenant_id extraction to JWT middleware
```

Display the prepared message to the user. Do not commit yet.

### 3b. Review diff

Run and display the staged diff:

```bash
git diff --cached
```

Ask the user:

> "Revise o diff acima. Confirma o commit? (sim/não)"

**If the answer is not "sim" — STOP.** Do not proceed. Inform:
> "Commit cancelado. Corrija o que for necessário e rode /task-done novamente."

### 3c. Append to SESSION_LOG.md

Only after explicit "sim" confirmation. Append a new session block to `SESSION_LOG.md` (in Portuguese):

```
## Sessão YYYY-MM-DD

**Data e hora de início:** [start of this session]
**Data e hora de fim:**    YYYY-MM-DD HH:MM
**Sprint:** [from CONTEXT.md]
**Branch:** [current git branch]
**Executado por:** Claude Code

### Tarefas Concluídas

| ID       | Descrição resumida | Score de Qualidade |
|----------|--------------------|--------------------|
| TASK-XXX | [description]      | X.X/10             |

### O que Falhou e Por Quê

[failures encountered, or "Nenhuma falha registrada."]

### Decisões Tomadas

[decisions made during this session, or "Nenhuma decisão nova."]

### Próximo Passo

**Tarefa:** [next PRONTO task from BACKLOG.md]
**Observação:** [relevant context for the next session]
```

### 3d. Update BACKLOG.md

In `BACKLOG.md`, change the completed task's status:

```
**Status:** EM PROGRESSO  →  **Status:** CONCLUIDO
```

Also check off all DoD items that were satisfied.

### 3e. Commit

```bash
git add [only files modified by this task + SESSION_LOG.md + BACKLOG.md]
git commit -m "[message from 3a]"
```
