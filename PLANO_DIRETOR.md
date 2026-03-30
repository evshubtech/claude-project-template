---

# 📑 Plano Diretor: Auditoria e Reestruturação do Fluxo IA (v1.1)

---

## 1. Diagnóstico da Auditoria (Estado Atual)

### Inventário de Ativos
- **Controle (Claude Code):** Comandos (/new-feature, /pre-commit) e Skills (security, development).
- **Estratégia (Gemini CLI/Web):** Skills de arquitetura e auditoria global.
- **Templates:** Bases para CLAUDE.md e GEMINI.md.

### Gaps e Inconsistências Identificados
1. **Fragmentação de Segurança:** Regras de segurança espalhadas entre 3 arquivos diferentes, facilitando negligências.
2. **Perda de Memória:** O fluxo /agents-sync é manual e propenso a falhas; não há registro do "porquê" das mudanças entre sessões.
3. **Tarefas Monolíticas:** Falta uma definição de "Tarefa Atômica", levando o Claude Code a tentar mudanças grandes demais.
4. **Silêncio sobre LGPD:** Nenhuma regra de classificação de dados ou privacidade por design implementada.
5. **Validação Fraca:** O processo de commit atual aceita avisos de segurança sem bloquear o fluxo.

---

## 2. Novo Fluxo de Trabalho: "Contexto em Cascata"

O objetivo é garantir que o Claude Code nunca comece uma tarefa sem saber exatamente onde o Gemini parou.

### Hierarquia de Granularidade
1. **Épico** (Gemini Web): Definição de Produto e Regras de Negócio.
2. **Feature** (Gemini Web/CLI): Decomposição em tarefas técnicas e ADRs.
3. **Tarefa Atômica** (Claude Code): Execução técnica.
   - **Regra de Ouro:** Máximo 1 responsabilidade funcional e alteração de no máximo 1 arquivo principal de lógica por tarefa.

### Papéis dos Agentes
| Agente | Papel | Responsabilidade |
|---|---|---|
| Gemini Web | Arquiteto + QA | Planejamento, breakdown, scoring de tarefas críticas |
| Claude Code | Engenheiro Sênior | Implementação de tarefas atômicas |
| Claude.ai | Consultor Estratégico | Contexto, processo, decisões arquiteturais difusas |
| Gemini CLI | Auditor | Análise de codebase completo, segurança OWASP |

### Arquivos de Memória Viva (por projeto — não no template)
- **CONTEXT.md:** Estado atual do projeto (Sprint, decisões críticas, bloqueios). Norte para qualquer IA que entre no projeto.
- **BACKLOG.md:** Lista de Tarefas Atômicas geradas pelo Gemini. O Claude Code consome daqui.
- **SESSION_LOG.md:** Registro em Português de cada sessão (append). Contém: o que foi feito, o que falhou, próximo passo, branch.

> ⚠️ Esses arquivos existem apenas nos repositórios de produto (FlowOps, CORA).
> O repositório template não possui SESSION_LOG.md — melhorias no template não precisam de rastreamento de sessão.

### Tipos de Sessão
- **Sessão de produto:** entrega feature/bug/melhoria → roda `/task-done` completo (scoring + SAST + commit)
- **Sessão de infraestrutura/setup:** entrega arquivos de configuração, templates, comandos → validação manual + Gemini revisa + commit direto

### Regra de Idioma
| Contexto | Idioma |
|---|---|
| Código (variáveis, funções, schemas, commits, nomes de arquivo) | Inglês |
| Instruções para modelos (skills, commands, CLAUDE.md, GEMINI.md) | Inglês |
| Prompt no BACKLOG.md (consumido pelo Claude Code) | Inglês obrigatório |
| Documentação de revisão humana (SESSION_LOG, CONTEXT, ADRs, critérios de aceite) | Português |
| BACKLOG.md (campos e estrutura / conteúdo preenchido) | Inglês / Português |

> Regra geral: se um humano vai ler pra decidir → português. Se um modelo vai ler pra executar → inglês.

### Definições de Prontidão e Conclusão
- **DoR (Ready):** Contexto injetado + Prompt em inglês com Critérios de Aceite + Classificação LGPD atribuída + dependências identificadas.
- **DoD (Done):** Todos os ACs satisfeitos + testes passando + zero vulnerabilidades SAST HIGH/CRITICAL + score >= 7 + SESSION_LOG atualizado.

---

## 3. Reestruturação de Arquivos (Plano de Ação)

| Arquivo | Ação | Finalidade |
|---|---|---|
| `.claude/skills/security.md` | MODIFICAR | Centralizar regras OWASP, multi-tenancy e auditoria. |
| `.claude/skills/privacy.md` | CRIAR | Regras LGPD, mascaramento de PII e retenção de dados. |
| `.claude/commands/pre-commit.md` | MODIFICAR | HARD BLOCK em falhas de segurança/lint. |
| `.claude/commands/session-start.md` | CRIADO ✅ | Injeta contexto no início de cada sessão de produto. |
| `.claude/commands/task-done.md` | CRIADO ✅ | Scoring + SAST + commit com confirmação explícita. |
| `CONTEXT.md.template` | CRIADO ✅ | Template de estado vivo do projeto. |
| `BACKLOG.md.template` | CRIADO ✅ | Template de tarefas atômicas com DoR/DoD/Prompt. |
| `SESSION_LOG.md.template` | CRIADO ✅ | Template de log de sessão (append, com campo Branch). |
| `template_claude.md` | MODIFICADO ✅ | Bloco de idioma + seção Security Tools adicionados. |

---

## 4. Segurança e Privacidade (LGPD por Design)

### Camada de Segurança (SAST)
- **Gate 2 no /task-done:** SAST agnóstico de linguagem. Ferramenta configurada no CLAUDE.md de cada projeto.
- **Exemplos por stack:** Python → `bandit -r ./src` | Node → `npm audit` | TypeScript → `tsc --noEmit`
- **Bloqueio:** vulnerabilidades HIGH ou CRITICAL interrompem o fluxo antes do commit.
- **Commit seguro:** /task-done exibe `git diff --cached` e aguarda confirmação explícita antes de executar.

### Estratégia LGPD (por produto)
- **FlowOps:** Base Legal para Execução de Contrato. Regras de exclusão de dados de funcionários ao fim do contrato.
- **CORA:** Dados Sensíveis (Art. 5, IX). Criptografia obrigatória para CPF/CREA, alertas de consentimento para dados de comunidades.
- **Classificação obrigatória por tarefa:** `[PUBLICO | INTERNO | PESSOAL | SENSIVEL]` — campo no BACKLOG.md.

### Próximas implementações (nos projetos, não no template)
- Skill de segurança centralizada (OWASP + multi-tenant) → primeira sessão do FlowOps
- Skill de privacidade LGPD → primeira sessão do CORA

---

## 5. Scoring de Qualidade (Dual)

- **Nível 1 — Auto-avaliação Claude** (toda tarefa):
  - Cobertura de Testes: peso 3
  - Segurança/Privacidade: peso 4
  - Aderência ao Estilo/DRY: peso 3
  - Score < 7 → gera brief de reinício, bloqueia commit

- **Nível 2 — Auditoria Gemini** (tarefas críticas: Auth, Data Migrations):
  - Gemini CLI faz double-check antes do merge

---

## 6. Decisões Validadas

| # | Decisão | Status |
|---|---|---|
| 1 | Logs de sessão em Português | ✅ |
| 2 | Commits bloqueados em falhas de auditoria | ✅ |
| 3 | CORA: stack definida após mapeamento de negócio | ✅ |
| 4 | Scoring dual (Claude + Gemini) | ✅ |
| 5 | Commit: exibe diff + aguarda confirmação explícita | ✅ |
| 6 | Backlog vazio no /session-start: avisa e para | ✅ |
| 7 | SESSION_LOG.md: append por sessão, mesmo arquivo | ✅ |
| 8 | /task-done e /session-start: separados por etapas | ✅ |
| 9 | Template não possui SESSION_LOG.md | ✅ |
| 10 | Arquivamento do SESSION_LOG.md: manual a cada ~50 sessões | ✅ |
| 11 | Sessões 3 e 4 (skills de segurança e LGPD) executar nos projetos, não no template | ✅ |

---

## 7. Decisões Futuras (fora do escopo atual)

- Comandos equivalentes ao /session-start e /task-done para o Gemini CLI
  → Avaliar junto com MCPs
- MCPs: Supabase (validação de schema/RLS) e GitHub (criação de PRs)
  → Avaliar após template estável nos projetos
- DAST com OWASP ZAP no checklist de deploy
  → Avaliar após primeiras versões em staging
- Arquivamento automático do SESSION_LOG.md
  → Reavaliar após ~50 sessões de uso real

---

## 8. Próximos Passos

### Template (concluído)
- [x] Sessão 1: templates de memória viva + comandos /session-start e /task-done
- [x] Sessão 2: correções pós-revisão do Gemini

### Projetos (a iniciar)
1. Copiar template para FlowOps e CORA
2. Preencher CLAUDE.md e GEMINI.md de cada projeto com stack real
3. Configurar Security Tools no CLAUDE.md de cada projeto
4. FlowOps — Sessão 3: skill de segurança centralizada (OWASP + multi-tenant)
5. CORA — Sessão 3: skill de privacidade LGPD + mapeamento de dados sensíveis

---

*Documento mantido por Everton. Última atualização: 2026-03-29.*
*Gerado inicialmente pelo Gemini CLI como Arquiteto Sênior.*