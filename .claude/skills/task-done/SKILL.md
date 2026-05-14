---
name: task-done
description: Runs the completion flow for a task — quality scoring, SAST, ADR check, then commit. Invoke with /task-done when you believe a task is fully implemented.
---

# /task-done

Usage: `/task-done TASK-XXX`

## Step 0 — Verify task status

Read `BACKLOG.md` and locate the task matching the provided ID.

If not found, stop and report:
> "Task [TASK-XXX] not found in BACKLOG.md. Check the ID and try again."

If the task status is not `EM PROGRESSO`, stop and report:
> "Task [TASK-XXX] has status [current status]. Only EM PROGRESSO tasks can be finished via /task-done. Make sure you used /session-start or manually marked it as EM PROGRESSO before starting implementation."

**Gate 0 Assumption:**
This command assumes Gate 0 (Gemini `/review`) has been completed and approved by the user. Proceed only after Gemini approval.

Confirm to the user:
> "Task [TASK-XXX] verified as EM PROGRESSO. Running quality gates…"

---

Sequential gates. Each gate can block progress.

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
> Stop here. Do not proceed to Gate 2.5.

---

## Gate 2.5 — ADR (Architectural Decision Record)

Ask the user:
> "Esta task gerou uma decisão arquitetural significativa? (sim/não)"

**If yes:**
1. Generate an ADR block following the `architecture` skill format:
   - **Status**: Accepted
   - **Context**: Reason for the decision in this task
   - **Decision**: What was decided/implemented
   - **Consequences**: Identified trade-offs
2. Identify the ADR path (configured in `ADR_PATH` in CLAUDE.md, default: `docs/decisions/ADR-[N].md`).
3. Create/update the file with the new ADR block.
4. Inform the user:
   > "ADR gerado e salvo em [path]. Proseguindo para o Gate 3..."

**If no:**
   > "Nenhuma decisão arquitetural registrada. Proseguindo para o Gate 3..."

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

Only after explicit "sim" confirmation. Append a new session block to `SESSION_LOG.md` (in Portuguese) prefixing the title with `[CLAUDE]`:

```
## [CLAUDE] Sessão YYYY-MM-DD HH:MM

**Épico:** [from CONTEXT.md]
**Branch:** [current git branch]
**Tarefa:** TASK-XXX — [description]
**Score Gate 1:** X.X/10
**Executado por:** Claude Code

### Implementado

[Summary of implementation]

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
