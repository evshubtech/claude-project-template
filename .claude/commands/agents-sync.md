---
name: agents-sync
description: Generate a context update block to synchronize both Gemini Web and Claude Web anchor chats. Invoke with /agents-sync after significant architectural changes, new ADRs, or stack updates.
---

# /agents-sync

Generates a unified, high-density context update block to be pasted into the **Gemini Web** (Architect) and **Claude Web** (Product/Implementation) anchor chats. This ensures both agents are working from the latest codebase state and architectural decisions.

## When to Use

- After a major infrastructure change (e.g., Database migration, SSL setup)
- After accepting a new Architectural Decision Record (ADR)
- When a new module or bounded context is introduced
- After updating `GEMINI.md` or `CLAUDE.md` with strategic information
- When the project scope or constraints change materially

## What to Include in the Sync Block

Generate the following block based on the current state of `CLAUDE.md`, `GEMINI.md` and recent changes:

```text
---
🔄 AGENTS SYNC — [Nome do Projeto] — [Data]
---

## 1. Stack & Infraestrutura (Mudanças Recentes)
- [Mudança 1: ex: Migrado para Supabase Session Pooler (Porta 5432)]
- [Mudança 2: ex: SSL forçado com validação de certificado CA]
- [Status: ex: Dev local sincronizado; Pronto para MVP no Replit]

## 2. Decisões Arquiteturais Recentes (ADRs)
| ADR | Título | Status |
|-----|--------|--------|
| ADR-0NN | [Título] | Aceito |

## 3. Evolução do Produto & Roadmap
- [Funcionalidade X: ex: Fundação da Central de Ajuda (Melhoria #018) adicionada]
- [Funcionalidade Y: ex: Integração com Resend para e-mails de suporte]

## 4. Snapshot da Arquitetura Atual
[Breve descrição do fluxo atual, ex: React + Express + Drizzle sobre Supabase]

## 5. Contexto para Próximas Sprints
[O que os agentes devem focar a seguir, ex: Checklist de deploy Replit, guias da Central de Ajuda]

---
Instrução: Por favor, atualize seu modelo mental com este bloco. Não sugira padrões obsoletos (ex: Neon ou conexões sem SSL).
---
```

## Instructions

1. Gather context using:
   ```bash
   cat GEMINI.md && echo "---" && git log --oneline -10
   ```

   and 

   ```bash
   cat CLAUDE.md && echo "---" && git log --oneline -10
   ```

2. Fill out the template above based on the gathered information.

3. **Paste the generated block into both Gemini Web and Claude Web chats.**

4. Update `CLAUDE.md` and `GEMINI.md` with the current date and `Sincronizado com chats âncora: sim` before closing.
