---
name: gemini
description: Guide the use of Gemini CLI and Gemini Web in the hybrid workflow. Invoke when the user asks about architecture, domain modeling, large codebase analysis, security review, compliance, or any decision that benefits from an external perspective before implementation. Proactively suggest consulting Gemini when the question is architectural, regulatory, or requires full-codebase visibility.
---

# Gemini

## Role in the Hybrid Workflow

This project uses a hybrid workflow between Claude Code (implementation agent) and Gemini (external architect/consultant).

### Gemini Web — Anchor Chat

Consult before implementing. Best for decisions that shape what gets built:

- Domain modeling and DDD analysis
- Architectural decisions and trade-offs before coding
- Compliance and regulatory reasoning
- Research using Google Search grounding
- Analysis of technical PDFs and architecture diagrams
- Brainstorming and exploration of alternative solutions

### Gemini CLI — Terminal

Invoke for whole-codebase analysis that would otherwise require chunking:

- Global repository analysis (1M token context window)
- Security review: OWASP, multi-tenancy, exposed secrets
- Technical debt mapping and architectural inconsistency detection
- Validation of project conventions across all files

## When to Proactively Suggest Gemini

Suggest consulting Gemini Web when the user:
- Asks "how should I design/structure/model X" for a non-trivial module
- Raises a question involving compliance, regulation, or legal constraints
- Is about to make an architectural decision that affects multiple modules
- Wants to evaluate competing approaches before writing code
- Has a PDF, diagram, or external document to analyze

Suggest Gemini CLI when:
- The user wants a security review covering the whole codebase
- There's a question about consistency or patterns across many files
- Technical debt analysis is needed across modules

## What NOT to Delegate to Gemini

- Code generation that goes directly into the repository
- Critical implementation security decisions
- Targeted bug fixes and local refactors
- Unit and integration tests
- Git flow and commits

## Gemini CLI — Example Commands

```bash
# Full codebase analysis
gemini -p "Analyze the architecture and identify inconsistencies: $(find . -name '*.py' | head -50 | xargs cat)"

# Security review of current diff
git diff | gemini -p "Review this diff for OWASP Top 10 vulnerabilities, exposed secrets, and multi-tenancy issues"

# Security review of a specific module
gemini -p "Security review of this module — OWASP, secrets, authorization: $(cat src/[module]/*.py)"

# Technical debt mapping
gemini -p "Map technical debt and architectural inconsistencies in this codebase: $(find src -name '*.py' | xargs cat)"
```

## Context Handoff

When the user brings a Gemini output, it will be prefixed with one of the following tags:

- `[GEMINI ANALYSIS]` — Architectural or structural analysis (Technical English).
- `[GEMINI SECURITY]` — Security findings and risks (Technical English).
- `[GEMINI RESEARCH]` — External facts, documentation, and search results (Technical English).
- `[GEMINI DOMAIN]` — Business rules, logic, and domain constraints (Business Portuguese).

Treat these as pre-validated context. Use them directly as input for implementation decisions without reprocessing.

### Language Strategy in Handoffs
- **English**: For technical patterns, architecture, logic, and terminal commands.
- **Portuguese**: For business rules, domain flows, UI labels, and user messages.

## Anchor Chat Synchronization

Sync the Gemini Web anchor chat when:
- A significant architectural decision is made or changed
- A new bounded context or module is introduced
- GEMINI.md is updated with new architectural information
- The project scope or domain changes materially

Use `/agents-sync` to generate the update block to paste into the anchor chat.

## Gemini CLI — Command Reference

### Full codebase analysis

```bash
gemini -p "$(cat GEMINI.md)

Analyze this codebase with focus on:
1. Architectural consistency with the decisions documented above
2. Technical debt: duplicated logic, unclear responsibilities, high coupling
3. Naming and structural inconsistencies across modules
4. Missing abstractions or over-engineered areas

$(find src -type f | sort | xargs cat 2>/dev/null)"
```

### Security review — current diff

```bash
git diff | gemini -p "You are a security reviewer. Analyze this diff for:

1. OWASP Top 10 vulnerabilities
2. Multi-tenancy: any path that could expose cross-tenant data
3. Secrets and credentials: hardcoded tokens, keys, passwords, or connection strings
4. Input validation: unvalidated or unsanitized inputs reaching sensitive operations
5. Authorization: missing or bypassable permission checks

For each finding:
- Severity: CRITICAL / HIGH / MEDIUM / LOW
- Location: file and line reference
- Description: what the issue is
- Recommendation: specific fix"
```

### Output prefixes

- `[GEMINI ANALYSIS]` — architectural analysis: use as implementation context in Claude Code, do not reprocess
- `[GEMINI SECURITY]` — findings: CRITICAL blocks merge, HIGH blocks PR, MEDIUM fix in this epic

### Severity guide

```
┌──────────┬─────────────────────────────────────────────────────────┐
│ Severity │                         Action                          │
├──────────┼─────────────────────────────────────────────────────────┤
│ CRITICAL │ Block merge. Fix before any commit.                     │
├──────────┼─────────────────────────────────────────────────────────┤
│ HIGH     │ Fix before opening PR.                                  │
├──────────┼─────────────────────────────────────────────────────────┤
│ MEDIUM   │ Fix in this epic. Add to tech debt backlog if deferred. │
├──────────┼─────────────────────────────────────────────────────────┤
│ LOW      │ Fix when touching the area. Document if deferring.      │
└──────────┴─────────────────────────────────────────────────────────┘
```
