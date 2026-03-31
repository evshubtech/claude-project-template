---
name: review
description: Run a full pre-PR review checklist covering security, correctness, tests, and documentation. Invoke with /review before opening any pull request.
---

# /review

Runs the full code-review skill against the current diff or specified files.

## Usage
```
/review                     # reviews uncommitted changes
/review src/module/file.py  # reviews a specific file
```

## What it does
Loads the `code-review` skill and systematically checks:
1. Security — secrets, input validation, authorization
2. Correctness — logic, edge cases, error handling
3. Code quality — structure, naming, duplication
4. Tests — coverage, regressions
5. Data — queries, migrations (if applicable)
6. Cloud / Infra — Terraform, IAM (if applicable)
7. Documentation — docstrings, README, CHANGELOG
8. General — debug code, commit messages, focused diff

## Output
A checklist with ✅ / ❌ / ⚠️ per item, plus a summary of blockers before merge.
