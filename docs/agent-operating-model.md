# Agent Operating Model

This repository is a reusable AI engineering template. Its agent structure is intentionally additive: Claude Code, Gemini CLI, Codex CLI, and future tools share the same project memory instead of competing for separate sources of truth.

## Current Weaknesses for Codex CLI

- No Codex-specific project context file existed, so Codex had to infer role, command boundaries, and closeout behavior from Claude/Gemini instructions.
- Root `AGENTS.md` was a useful charter but too small to act as an operational entrypoint.
- Existing workflows named Claude as the only implementation executor, even though the memory model can support multiple executors.
- Skill hierarchy was tool-specific. Codex needed workflow skills for session start and task closeout, plus small component skills for context loading, atomic execution, diff review, and closeout.
- Context-loading rules did not distinguish between planning memory, execution memory, and repository-local code context.

## Evolution Plan

1. Keep Gemini as architect/auditor for discovery, ADRs, breakdown, and broad review.
2. Keep Claude Code workflows intact for projects that rely on Claude commands and hooks.
3. Add Codex as a first-class implementation executor that consumes the same `CONTEXT.md`, `BACKLOG.md`, and `SESSION_LOG.md`.
4. Prefer two explicit Codex workflow skills (`session-start`, `task-done`) backed by small component skills instead of one monolithic instruction file.
5. Use root and subdirectory `AGENTS.md` files for stable repository rules, while putting reusable procedures in skills.

## Why This Benefits Codex

Codex performs best when it has:
- A clear edit boundary.
- Direct access to local files and validation commands.
- Small reusable procedures for recurring work.
- A reliable context-loading order.
- Explicit rules for preserving dirty worktrees and avoiding destructive operations.

The `.codex/skills` hierarchy provides those behaviors without requiring each prompt to restate them. The recommended execution flow is:

```text
session-start -> atomic-task -> Gemini /review -> task-done
```

## Tradeoffs

- Adding `CODEX.md` increases the number of top-level context files, but avoids overloading `AGENTS.md` with tool-specific details.
- Codex workflow skills overlap conceptually with Claude commands, intentionally. The overlap preserves the developer's operating rhythm, while the internals stay Codex-native and composable.
- Shared executor roles can create ambiguity. The mitigation is to treat Gemini as the default strategic owner and Claude/Codex as execution owners, with the user choosing the active executor per session.

## Interoperability Impact

- Claude and Gemini workflows remain valid.
- `BACKLOG.md` tasks can now be consumed by either Claude Code or Codex CLI.
- `SESSION_LOG.md` gains `[CODEX]` as an allowed implementation-session prefix.
- `AGENTS.md` becomes the model-agnostic entrypoint for all agents.
- Future agents can reuse the same pattern: root instructions for stable rules, skills for reusable behavior, and shared memory for project state.
