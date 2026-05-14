---
name: implementation-orchestrator
description: Guidelines for generating high-efficiency prompts for execution agents (Claude Code and OpenAI Codex). Use this to format architectural or technical instructions intended for executor implementation.
---

# Implementation Orchestrator

## Goal
Generate surgical, token-efficient directives for the active execution agent (**Claude Code** or **OpenAI Codex**). Act as the "bridge" between Gemini's strategic analysis and the tactical implementation performed by the executors.

## Executor Selection (Flexible)
- **Claude Code**: Prefer for complex, multi-file refactoring, deep context reasoning, and tasks requiring high architectural consistency.
- **OpenAI Codex**: Prefer for specific algorithmic challenges, test generation, terminal-driven refactoring, and focused incremental patches.
- *Note: Guidelines should remain largely executor-agnostic unless a specific agent strength is required.*

## Tag Protocol
The following tags must be used to prefix outputs for **both** executors to ensure consistent context handoff:
- `[GEMINI ANALYSIS]`: Architectural patterns, refactoring, structure.
- `[GEMINI SECURITY]`: OWASP, multi-tenancy, secrets, authorization.
- `[GEMINI RESEARCH]`: Google Search results, documentation, API updates.
- `[GEMINI DOMAIN]`: Business logic, Portuguese rules, domain constraints.

## Language Strategy
- **English**: Technical directives, patterns, architecture, and logic. (Consumable by models).
- **Português**: Regras de negócio, domínio, labels de interface e termos de negócio. (For human review and domain fidelity).

## Prompt Structure (Agnostic)
1. **Tag Prefix**
2. **Context (English)**: Brief technical "why".
3. **Technical Directives (English)**: Imperative steps (Refactor, Implement, Fix).
4. **Regras de Negócio (Português)**: Domain fidelity and business constraints.
5. **Files to Modify**: Explicit list of target files.
6. **Validation (English)**: Commands to verify the change (e.g., specific test or lint commands).

## Optimization
- No fluff or conversational filler.
- Be surgical: only affect what is necessary.
- Provide explicit names for classes, methods, and variables to minimize ambiguity across different model architectures.
