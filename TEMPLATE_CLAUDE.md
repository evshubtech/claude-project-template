# [Project Name]

## Stack
- Language: [ex: Python / TypeScript]
- Framework: [ex: FastAPI / Express.js]
- Database: [ex: PostgreSQL / BigQuery]
- Cloud: [ex: AWS / GCP]
- IaC: Terraform

## Commands
- `[dev command]`
- `[test command]`
- `[build command]`
- `[lint command]`

## Security Tools

SAST tool for this project: <!-- ex: bandit / npm audit / tsc -->
Run command: <!-- ex: bandit -r ./src -->
Block threshold: HIGH, CRITICAL

## Mandatory Workflow Directives

1. **Skill Discovery**: Before starting any non-trivial task, you MUST check the `.claude/skills/` directory and load the relevant expert skill(s). A task is "non-trivial" if it involves architecture, security, data modeling, cloud infra, or complex business logic.
2. **Plan Mode Usage**: For complex or non-trivial tasks (new features, multi-file refactors), you are STRONGLY ENCOURAGED to use `plan_mode` to research and map dependencies before implementation. Tasks marked with `[STRATEGIC]` in `BACKLOG.md` MANDATE the use of Plan Mode.
3. **Interactive Commands**: NEVER execute commands that require terminal confirmation or user input (e.g., `db:push`, `terraform apply`, interactive migrations). Provide the exact command and ask the user to run it.
4. **User Validation**: BEFORE every commit, you MUST ask the user to manually test and validate the changes. Always state the prerequisites (what must be running: dev server, build, containers) BEFORE listing the test steps. Provide clear instructions on **what** to verify and **how** (paths, expected outputs, test scripts).
5. **Commit Protocol**: A task is only complete AFTER user validation and Gate 0 approval from Gemini CLI. Do not commit without explicit confirmation.
6. **Error Recovery**: If an execution error occurs (build failure, test crash, command error), you are allowed a maximum of **2 autonomous fix attempts**. If the 3rd attempt also fails, stop and generate a 'Gemini Debug Brief' containing: 1) The error log, 2) Root cause hypothesis, 3) List of previous failed attempts. This brief is for the user to consult Gemini CLI.

## Conventions
- Commits: Conventional Commits (feat:, fix:, chore:, docs:, refactor:)
- Branches: feature/, fix/, chore/ prefixes
- Code language: English
- Docs language: [your preference]

## Do Not Touch
- Migration files already applied
- [other critical files/folders]

## Context Loading Order

Load context in this order unless the user asks for a narrower task:

1. `AGENTS.md`
2. `CLAUDE.md` (this file)
3. `CONTEXT.md` — current project state
4. `BACKLOG.md` — active atomic task
5. Latest relevant block in `SESSION_LOG.md`
6. Relevant skills only (`.claude/skills/`)
7. Target source files and tests

Do not load every skill or every memory file by default. Prefer targeted reads and summarize large context before acting.

---

## Subdirectory AGENTS.md Pattern

Use subdirectory `AGENTS.md` files only when local rules differ from the root. Good candidates:

- `src/AGENTS.md`: module boundaries, test commands, domain constraints.
- `tests/AGENTS.md`: fixture rules, test style, data handling.
- `infra/AGENTS.md`: Terraform/state safety, cloud account boundaries.
- `docs/AGENTS.md`: ADR conventions and documentation language.

Subdirectory instructions should be short, operational, and scoped to that tree. Put reusable procedures in skills, not in every directory instruction file.

---

## Hybrid Workflow — Gemini CLI

This project uses a multi-agent workflow. See `GEMINI.md` and `AGENTS.md` for the full technical context. The user is the bridge between agents — Claude never calls Gemini or Codex directly.

- **Gemini CLI:** Architect & Auditor — planning, discovery, ADRs, security review.
- **Claude Code:** Senior Engineer — implementation, tests, commit gates.
- **Codex CLI:** Operational Executor — terminal-driven implementation, diff review, focused patches.

### Gemini CLI role

Invoke for planning, architectural decisions and deep codebase analysis:
- `/discovery` — domain elicitation and business rules
- `/breakdown` — feature decomposition into atomic tasks
- `/adr` — architectural decision records
- `/review` — pre-commit technical review (Gate 0)
- `/sync` — update CONTEXT.md and SESSION_LOG.md
- `/gemini-analyze` — global repository analysis (see `.claude/skills/gemini.md` for commands)
- `/gemini-security` — broad security review (see `.claude/skills/gemini.md` for commands)

### When to actively suggest Gemini CLI

- User asks how to structure something non-trivial
- Architectural decision affecting multiple modules
- Compliance or regulation question
- Broad codebase security review needed
- Inconsistencies across many files

### How to handle prefixed outputs

- `[GEMINI ANALYSIS]` — architectural analysis: use directly as implementation context, do not reprocess
- `[GEMINI SECURITY]` — findings: CRITICAL blocks merge, HIGH blocks PR
- `[GEMINI DOMAIN]` — business rules in Portuguese: follow the defined domain logic

The user has already validated the output before bringing it — do not question the source, implement from the received context.

### Available commands

- `/gemini-analyze` — global or module analysis via Gemini CLI
- `/gemini-security` — security review of diff or module
- `/sync` — update CONTEXT.md and SESSION_LOG.md

## Known Errors

<!-- Format: ### Error title / Cause / Solution -->
<!-- Only add real errors encountered during development — no assumptions -->

## Language

- **Code:** English (variables, functions, schemas, commits, filenames)
- **Model instructions** (skills, commands, CLAUDE.md, GEMINI.md, BACKLOG.md prompts): English
- **Human review docs** (SESSION_LOG.md, CONTEXT.md content, acceptance criteria, ADRs): Portuguese
- **BACKLOG.md:** hybrid — fields and structure in English, content filled in Portuguese
- **Rule of thumb:** if a human reads it to decide → Portuguese. If a model reads it to execute → English.

## Security Setup

MULTI_TENANT:
SECURITY_TOOL:
ADR_PATH:
AUTH_METHOD:

## Privacy Setup

LEGAL_BASIS:
DATA_SUBJECTS:
PII_FIELDS:
RETENTION:
REGULATORY:
