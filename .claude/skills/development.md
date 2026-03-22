---
name: development
description: Apply coding conventions and daily development standards. Use when writing new code, refactoring existing code, reviewing naming conventions, or when the user asks about code structure, function design, or language-specific best practices.
---

# Development

## General Principles
- Write code for humans first, machines second
- One function = one responsibility
- If it needs a comment to be understood, consider renaming or refactoring
- DRY — but don't abstract prematurely
- Fail fast and loudly in development, gracefully in production

## Code Structure
- Keep functions under 30 lines when possible
- Max file length: ~300 lines — if larger, consider splitting
- Group related logic: imports → constants → types → functions → exports
- No dead code — remove instead of commenting out

## Naming
- Variables and functions: descriptive, intention-revealing
- Booleans: `is_`, `has_`, `should_` prefixes
- Avoid abbreviations unless universally known (e.g. `id`, `url`)

## Error Handling
- Never swallow exceptions silently
- Always include context in error messages
- Use custom exception classes for domain errors
- Log errors with enough context to reproduce

## Before Submitting Code
- Self-review the diff before asking for review
- Remove debug logs and temporary code
- Ensure no hardcoded values that should be config

## Commit Workflow
- **Always ask the user to test the changes before committing.** Never run `git commit` or `git add` without first pausing and asking: "Pode testar e me confirmar se está funcionando?"
- Only proceed with the commit after explicit user confirmation that the test passed.
