---
name: new-feature
description: Checklist and workflow for starting a new feature correctly. Invoke with /new-feature when beginning any new feature, to ensure scope is clear and the right skills are loaded before coding starts.
---

# /new-feature

## 1. Clarify scope
- [ ] Problem statement is clear
- [ ] Acceptance criteria defined (read: product skill)
- [ ] Out of scope explicitly stated
- [ ] Dependencies identified

## 2. Plan before coding
- [ ] List files to be created and modified
- [ ] Identify integration points
- [ ] Does this need a migration? (read: data skill)
- [ ] Does this touch cloud resources? (read: cloud + finops skills)
- [ ] Does this integrate with external services? (read: integrations skill)
- [ ] Any significant architectural decision? (read: architecture skill)

## 3. Branch
```bash
git checkout -b feature/[short-description]
```

## 4. Implement
- Follow development skill conventions
- Apply security skill throughout
- Write tests alongside code (qa skill)

## 5. Before opening PR
- Run /pre-commit
- Update CHANGELOG.md
- Update README if behavior changed
