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

When the user brings a Gemini output, it will be prefixed with:

- `[GEMINI ANALYSIS]` — architectural or codebase analysis
- `[GEMINI SECURITY]` — security findings

Treat these as pre-validated context. Use them directly as input for implementation decisions without reprocessing. The user already reviewed the output before bringing it here.

## Anchor Chat Synchronization

Sync the Gemini Web anchor chat when:
- A significant architectural decision is made or changed
- A new bounded context or module is introduced
- GEMINI.md is updated with new architectural information
- The project scope or domain changes materially

Use `/gemini-sync` to generate the update block to paste into the anchor chat.
