# [Project Name] - Codex Context

Use this file as `CODEX.md` in projects that use Codex CLI for implementation, refactoring, review, and workflow automation.

---

## Project Purpose

[Describe the product, target users, and expected outcome in 2-4 sentences.]

---

## Stack

- Language: [ex: Python / TypeScript]
- Framework: [ex: FastAPI / Next.js]
- Database: [ex: PostgreSQL / BigQuery]
- Cloud: [ex: AWS / GCP]
- IaC: [ex: Terraform / Pulumi / none]
- Tests: [ex: pytest / Jest / Vitest]

---

## Commands

- Dev: `[dev command]`
- Test: `[test command]`
- Focused test: `[focused test command pattern]`
- Build: `[build command]`
- Lint: `[lint command]`
- Type check: `[typecheck command]`
- Security: `[security command]`

If a command is unknown, Codex should inspect project files first, then ask the user only when it cannot infer a safe command.

---

## Mandatory Workflow Directives

1. **Session start:** For backlog-driven work, start with `.codex/skills/session-start/SKILL.md`.
2. **Context loading:** Use `.codex/skills/context-load/SKILL.md` as the reusable context-loading component.
3. **Atomic execution:** Work from one `BACKLOG.md` task or one explicit user request at a time.
4. **Task closeout:** For completed backlog work, use `.codex/skills/task-done/SKILL.md` after Gemini Gate 0 when required.
5. **Preserve local changes:** Do not revert, overwrite, or reformat unrelated files.
6. **Before edits:** State the expected edit boundary and validation plan.
7. **After edits:** Run focused validation when available and review the diff.
8. **No silent commits:** Prepare commit messages when useful, but commit only after explicit user request.
9. **Interactive commands:** Do not run commands that require confirmation, production credentials, irreversible migrations, or external side effects without explicit approval.
10. **Security gate:** Block closeout on HIGH/CRITICAL SAST findings when a security command is configured.

---

## Codex Role

Codex CLI is an implementation executor and workflow-aware engineering agent.

Codex should own:
- Local code changes, refactors, tests, and docs updates.
- Terminal-driven investigation and validation.
- Focused review of current diffs.
- Maintaining shared context files when working inside an instantiated project.
- Translating Gemini architectural output into concrete patches.

Codex should not own by default:
- Broad product discovery.
- Multi-module architecture decisions without user or Gemini validation.
- External research unless the user asks for it.
- Commits, pushes, deploys, or destructive operations without explicit confirmation.

---

## Hybrid Workflow

This project supports Claude Code, Gemini CLI, Codex CLI, and future agents.

### Gemini CLI

Use Gemini for:
- `/discovery`: domain elicitation and business rules.
- `/breakdown`: atomic task generation.
- `/adr`: architectural decision records.
- `/review`: architecture/security review for critical work.
- `/sync`: shared context updates after planning.

### Claude Code

Use Claude for:
- Existing Claude-native commands and skills.
- Projects already standardized on `/session-start` and `/task-done`.
- Commit-gated flows that depend on Claude hooks.

### Codex CLI

Use Codex for:
- Repository inspection and targeted implementation.
- Focused edits with local validation.
- Diff review and cleanup.
- Template evolution and cross-agent interoperability work.

Codex consumes the same `CONTEXT.md`, `BACKLOG.md`, and `SESSION_LOG.md` files as Claude and Gemini.

Recommended Codex task flow:

```text
session-start -> atomic-task -> Gemini /review -> task-done
```

The workflow skills are intentionally thin orchestration layers. `session-start` composes `context-load`; `task-done` composes `review-diff` and `task-closeout`.

---

## Context Strategy

Load context in this order:

1. `AGENTS.md`
2. `CODEX.md`
3. `CONTEXT.md`
4. `BACKLOG.md`
5. Latest block from `SESSION_LOG.md`
6. Relevant source files and tests
7. `.codex/skills/session-start/SKILL.md` or `.codex/skills/task-done/SKILL.md` when running those workflows
8. Relevant component skills such as `context-load`, `atomic-task`, `review-diff`, and `task-closeout`

Avoid loading every skill by default. Choose only the smallest skill set needed for the task.

---

## Recommended AGENTS.md Hierarchy

Use subdirectory `AGENTS.md` files when a folder has local rules that should override or refine root behavior.

Suggested structure:

```text
AGENTS.md
src/AGENTS.md
tests/AGENTS.md
infra/AGENTS.md
docs/AGENTS.md
```

Subdirectory files should define:
- Ownership and responsibility of that folder.
- Local validation commands.
- Files that agents should not touch casually.
- Domain-specific constraints.
- Links to relevant skills or ADRs.

Keep each `AGENTS.md` short. Put reusable procedures in skills, not in every directory instruction file.

---

## Language Rules

- Code, identifiers, filenames, commits, model-facing skills: English.
- Human decision records, ADRs, `SESSION_LOG.md`, `CONTEXT.md`, acceptance criteria: Portuguese.
- `BACKLOG.md`: English field names and executor prompts; Portuguese human-facing content.

When unsure: if a model executes it, write English. If a human decides from it, write Portuguese.

---

## Security and Privacy

- MULTI_TENANT: [true | false]
- SECURITY_TOOL: [ex: npm audit && tsc --noEmit]
- ADR_PATH: [ex: docs/decisions/]
- AUTH_METHOD: [ex: JWT / session / OAuth]
- SENSITIVE_DATA: [ex: email, CPF, health data]
- LEGAL_BASIS: [ex: contrato | consentimento | legitimo interesse | obrigacao legal]
- PII_FIELDS: [ex: nome, email, cargo]
- RETENTION: [ex: vigencia do contrato + 5 anos]

Codex must not log secrets or personal data and must flag tasks that conflict with the declared LGPD classification.
