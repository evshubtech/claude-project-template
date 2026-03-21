---
name: gemini-sync
description: Generate an update block to synchronize the Gemini Web anchor chat. Invoke with /gemini-sync after significant architectural changes, new modules, or updates to GEMINI.md.
---

# /gemini-sync

Generates a structured update block to paste into the Gemini Web anchor chat, keeping the external architect in sync with the current state of the project.

## When to Use

- After accepting a significant architectural decision (new ADR)
- When a new bounded context or module is introduced
- After updating `GEMINI.md` with new architectural information
- When the project scope, domain, or stack changes materially
- After a `/gemini-analyze` session surfaces structural changes worth anchoring

## What to Include in the Sync Block

Generate the following block based on the current state of `GEMINI.md` and recent changes:

```
[GEMINI SYNC] — [project name] — [date]

## O que mudou
- [mudança 1: ex: novo módulo de pagamentos adicionado]
- [mudança 2: ex: ADR-003 aceito — migração para event-driven no módulo X]

## Estado atual da arquitetura
[breve resumo do estado atual — copiável da seção de arquitetura do GEMINI.md]

## Decisões recentes
| ADR | Título | Status |
|-----|--------|--------|
| ADR-00N | [título] | Aceito |

## Contexto para próximas discussões
[o que está planejado ou em aberto que o Gemini web deve saber para as próximas consultas]

## GEMINI.md atualizado?
[ ] Sim — já reflete o estado atual
[ ] Não — atualizar antes de sincronizar
```

## Instructions

1. Run this command to get the current GEMINI.md content and recent git log:

```bash
cat GEMINI.md && echo "---" && git log --oneline -10
```

2. Use the output above to fill in the sync block template.

3. **Paste the generated block into the Gemini Web anchor chat** with a message like:
   > "Sincronizando contexto do projeto. Por favor, atualize seu entendimento do estado arquitetural com base neste bloco."

4. Update `GEMINI.md` with the current date and `Sincronizado com chat âncora: sim` before closing.
