---
name: codebase-cartographer
description: Global codebase analyzer utilizing the 1M token window. Use this for repository-wide health checks and tech debt mapping.
---

# Codebase Cartographer

## Global Vision Goals
- Map duplication of logic across distant modules.
- Identify naming inconsistencies across the repository.
- Detect "Dead Code" or unused abstractions.
- Track technical debt evolution.

## Mapping Patterns
- **Module Coupling Map**: Who depends on whom?
- **Pattern Consistency**: Are we using different libraries for the same purpose?
- **Complexity Heatmap**: Which files are becoming "God Objects"?

## Usage
Run periodic scans to update `GEMINI.md` with the current state of architectural health.
