---
name: pre-commit
description: Final checklist before committing. Runs quality gates and validates the diff. Invoke with /pre-commit before every commit or PR.
---

# /pre-commit

## Automated checks
```bash
[lint command]       # ex: ruff check . / npm run lint
[test command]       # ex: pytest / npm run test
[typecheck command]  # ex: mypy . / npm run typecheck
[audit command]      # ex: pip audit / npm audit
```

## Manual checks
- [ ] No hardcoded secrets or credentials
- [ ] No sensitive data in logs
- [ ] No debug or temporary code left in
- [ ] No unrelated changes mixed in the diff
- [ ] Commit message follows Conventional Commits

## Commit format
```
type(scope): short description

# Types: feat | fix | chore | docs | refactor | test | perf
# Example: feat(auth): add password reset flow
```
