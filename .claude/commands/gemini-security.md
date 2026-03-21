---
name: gemini-security
description: Run a security review via Gemini CLI. Invoke with /gemini-security to check the current diff or a specific module against OWASP Top 10, multi-tenancy issues, and exposed secrets.
---

# /gemini-security

Uses Gemini CLI to perform a security-focused review. Complements Claude Code's built-in security skill with full-codebase visibility.

## Usage

```
/gemini-security                       # reviews current uncommitted diff
/gemini-security src/payments          # reviews a specific module
```

## Commands

### Review current diff

```bash
git diff | gemini -p "You are a security reviewer. Analyze this diff for:

1. OWASP Top 10: injection, broken auth, sensitive data exposure, broken access control,
   security misconfiguration, XSS, insecure deserialization, known vulnerabilities, insufficient logging
2. Multi-tenancy: any path that could expose cross-tenant data
3. Secrets and credentials: hardcoded tokens, keys, passwords, or connection strings
4. Input validation: unvalidated or unsanitized inputs reaching sensitive operations
5. Authorization: missing or bypassable permission checks

For each finding, provide:
- Severity: CRITICAL / HIGH / MEDIUM / LOW
- Location: file and line reference
- Description: what the issue is
- Recommendation: specific fix"
```

### Review a specific module

```bash
gemini -p "You are a security reviewer. Analyze this module for:

1. OWASP Top 10 vulnerabilities
2. Multi-tenancy data isolation
3. Hardcoded secrets or credentials
4. Input validation and sanitization
5. Authorization and access control gaps

For each finding, provide:
- Severity: CRITICAL / HIGH / MEDIUM / LOW
- Location: file and line reference
- Description: what the issue is
- Recommendation: specific fix

$(find src/[module] -type f | sort | xargs cat)"
```

## Output Format

Present the Gemini output structured as:

```
[GEMINI SECURITY] — [scope reviewed] — [date]

## Critical
...

## High
...

## Medium
...

## Low
...

## Summary
X critical, X high, X medium, X low findings.
```

## Severity Guide

| Severity | Action |
|----------|--------|
| CRITICAL | Block merge. Fix before any commit. |
| HIGH | Fix before opening PR. |
| MEDIUM | Fix in this sprint. Add to tech debt backlog if deferred. |
| LOW | Fix when touching the area. Document if deferring. |

## Next Steps

- CRITICAL or HIGH findings: address immediately, then re-run `/gemini-security`
- Use the output as context when asking Claude Code to implement the fixes
- Recurring patterns should be added to CLAUDE.md Known Errors
- Prefix outputs with `[GEMINI SECURITY]` when bringing to Claude Code
