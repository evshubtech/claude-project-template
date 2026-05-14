# Repository Purpose

This repository is an evolving AI-native software engineering template.

The goal is to support:
- Codex CLI
- Claude Code
- Gemini CLI
- future agentic tooling

# Agent Roles

- Gemini CLI: architecture, discovery, ADRs, broad codebase analysis, and security review.
- Claude Code: Claude-native implementation workflows, commands, hooks, and commit gates.
- Codex CLI: terminal-driven implementation, refactoring, focused review, validation, and template evolution.
- Future agents: consume the same shared memory files and obey the same repository boundaries.

# Principles

- Model agnostic first
- Reusable patterns
- Operational simplicity
- Explicit workflows
- Small composable skills
- Minimal hidden context

# Context Loading Order

Agents should load context in this order unless the user asks for a narrower task:

1. `AGENTS.md`
2. Tool-specific context: `CODEX.md`, `CLAUDE.md`, or `GEMINI.md`
3. `CONTEXT.md` for current project state
4. `BACKLOG.md` for the active atomic task
5. Latest relevant block in `SESSION_LOG.md`
6. Relevant skills only
7. Target source files and tests

Do not load every skill or every memory file by default. Prefer targeted reads and summarize large context before acting.

# Agent Expectations

Agents should:
- prefer incremental changes
- avoid unnecessary rewrites
- preserve interoperability
- document structural reasoning
- optimize for future reuse
- preserve unrelated local changes
- validate changes with repository-native commands when available
- avoid commits, pushes, deploys, migrations, or destructive commands unless explicitly requested

# Skill Boundaries

- `.codex/skills/`: Codex-native reusable procedures.
- `.claude/skills/`: Claude-native reusable procedures and command-style workflows.
- `.gemini/skills/`: Gemini-native strategic analysis, architecture audit, domain modeling, UX review, and QA strategy procedures.

When adding new behavior, prefer the smallest tool-specific skill that can reuse shared project memory instead of embedding large workflows in this file.

# Subdirectory AGENTS.md Pattern

Use subdirectory `AGENTS.md` files only when local rules differ from the root. Good candidates are:

- `src/AGENTS.md`: module boundaries, test commands, domain constraints.
- `tests/AGENTS.md`: fixture rules, test style, data handling.
- `infra/AGENTS.md`: Terraform/state safety, cloud account boundaries.
- `docs/AGENTS.md`: ADR conventions and documentation language.

Subdirectory instructions should be short, operational, and scoped to that tree.
