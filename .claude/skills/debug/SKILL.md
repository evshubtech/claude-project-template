---
name: debug
description: Structured debugging workflow to avoid random trial and error. Invoke with /debug when investigating a bug, unexpected behavior, or production incident.
---

# /debug

## Process

### 1. Reproduce first
- Can you reproduce consistently? If not, gather more data before changing anything
- Document exact steps to reproduce
- Identify affected environment (dev / staging / prod)

### 2. Gather evidence
- Read the full error message and stack trace — don't skim
- Check logs around the time of failure
- Identify what changed recently (last deploy, config change, data change)

### 3. Form hypothesis
- State one hypothesis at a time
- What would prove or disprove it?
- Test the simplest hypothesis first

### 4. Isolate
- Reproduce in the smallest possible context
- Remove variables one at a time

### 5. Fix
- Fix the root cause, not the symptom
- What else could this fix break? (run /review after)
- Add a test that would have caught this bug

### 6. Document
- If non-obvious: add to CLAUDE.md Known Errors section
- If infra issue: update runbook
- If recurring pattern: create a task to address root cause

## Common Pitfalls
- Don't change multiple things at once
- Don't assume — verify with evidence
- Don't skip reading the full stack trace
- Don't fix in production directly — reproduce locally first
