---
name: code-review
description: Perform a systematic review before any PR or commit. Use when the user asks for a code review, is about to open a PR, or wants to validate changes across security, correctness, tests, and documentation. Invoke with /review or when the user says "review this" or "is this ready to merge".
---

# Code Review

## How to Use
Review each section below and confirm or flag issues found.

## 1. Security
- [ ] No hardcoded secrets, tokens, or credentials
- [ ] No sensitive data in logs
- [ ] All inputs validated and sanitized
- [ ] Authorization checks in place
- [ ] No SQL injection vectors

## 2. Correctness
- [ ] Logic handles all expected cases
- [ ] Edge cases considered (empty, null, zero, max values)
- [ ] Error handling is explicit — no silent failures
- [ ] Async operations properly awaited

## 3. Code Quality
- [ ] Functions have single responsibility
- [ ] No unnecessary duplication
- [ ] Names are clear and intention-revealing
- [ ] No dead code or commented-out blocks
- [ ] No `TODO` without a linked issue

## 4. Tests
- [ ] New code has tests
- [ ] Existing tests still pass
- [ ] Tests cover happy path and failure scenarios

## 5. Data (if applicable)
- [ ] No unbounded queries
- [ ] Migrations are safe and have rollback path
- [ ] No raw string interpolation in queries

## 6. Cloud / Infra (if applicable)
- [ ] Infrastructure changes are in Terraform
- [ ] No manual changes to cloud resources
- [ ] IAM permissions follow least privilege

## 7. Documentation
- [ ] Public functions have docstrings
- [ ] README updated if behavior changed
- [ ] CHANGELOG entry added for user-facing changes

## 8. General
- [ ] No debug/temporary code left in
- [ ] Diff is focused — no unrelated changes mixed in
- [ ] Commit messages follow Conventional Commits
