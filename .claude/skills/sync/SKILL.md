---
name: sync
description: Update project context (CONTEXT.md) and session logs (SESSION_LOG.md) based on the latest architectural decisions and task completions. Invoke with /sync at the end of a Gemini session or after significant changes.
---

# /sync

Synchronizes the project's living memory files to ensure all agents (Claude and Gemini) share the same mental model of the current state.

## When to Use

- At the end of any Gemini CLI session (discovery, breakdown, review).
- After a major architectural decision is accepted (ADR).
- After completing an epic or significant feature.
- When project blockers are identified or resolved.

## What it does

1. **Update CONTEXT.md**:
   - Refreshes the current epic objective.
   - Lists active decisions and critical blockers.
   - Sets the project "pulse" for the next sessions.

2. **Append to SESSION_LOG.md**:
   - Adds a summary of the work performed (Portuguese).
   - Records decisions made and failures encountered.
   - Defines the clear "Next Step" (TASK-XXX).

## Instructions

1. Read the current `CONTEXT.md` and `SESSION_LOG.md`.
2. Consolidate the session's outcomes.
3. Generate the update blocks in Portuguese for both files.
4. Apply the updates.
