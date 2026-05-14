---
name: review-diff
description: Review a Codex change or current diff for correctness, security, test coverage, and workflow compliance.
---

# Review Diff

Use this skill for pre-commit review, PR review, or user-requested review of local changes.

## Review Stance

Lead with findings. Focus on defects, regressions, missing tests, security/privacy issues, and workflow violations.

## Inputs

Inspect:
- `git status --short`
- `git diff --stat`
- `git diff`
- Relevant tests and instructions for touched files

## Checklist

- Scope: diff matches the task and avoids unrelated churn.
- Correctness: edge cases, failure paths, data shape changes, concurrency, idempotency.
- Security: secrets, injection, authorization, unsafe command execution, dependency risk.
- Privacy: LGPD classification respected, no personal data in logs, retention/masking rules preserved.
- Tests: meaningful coverage for happy path and failure path, or a clear reason tests are not applicable.
- Maintainability: names, duplication, dependency direction, unnecessary abstraction.
- Interoperability: no breakage to Claude/Gemini workflows or shared templates.

## Output Format

If issues exist:

```text
Findings
- [Severity] file:line - issue, impact, and fix direction.

Open Questions
- ...

Summary
- ...
```

If no issues exist, say so clearly and mention any unvalidated risk.
