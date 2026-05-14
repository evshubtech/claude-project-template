---

# 📑 Plano Diretor: Auditoria e Reestruturação do Fluxo IA (v1.1)

---

## 1. Diagnóstico da Auditoria (Estado Atual)

### Inventário de Ativos
- **Controle (Claude Code):** Comandos (/new-feature, /pre-commit) e Skills (security, development).
- **Execução (Codex CLI):** Skills pequenas para carregamento de contexto, tarefa atômica, revisão de diff e closeout.
- **Estratégia (Gemini CLI/Web):** Skills de arquitetura e auditoria global.
- **Templates:** Bases para CLAUDE.md, CODEX.md e GEMINI.md.

### Gaps e Inconsistências Identificados
1. **Fragmentação de Segurança:** Regras de segurança espalhadas entre 3 arquivos differentes, facilitando negligências.
2. **Perda de Memória:** Falta de um fluxo de sincronização contínuo; não há registro do "porquê" das mudanças entre sessões.
3. **Tarefas Monolíticas:** Falta uma definição de "Tarefa Atômica", levando o Claude Code a tentar mudanças grandes demais.
4. **Silêncio sobre LGPD:** Nenhuma regra de classificação de dados ou privacidade por design implementada.
5. **Validação Fraca:** O processo de commit atual aceita avisos de segurança sem bloquear o fluxo.
6. **Lacuna Codex:** Codex CLI não possuía entrada de contexto, skills nativas ou papel explícito no fluxo multiagente.

---

## 2. Novo Fluxo de Trabalho: "Contexto em Cascata"

O objetivo é garantir que o executor nunca comece uma tarefa sem saber exatamente onde o Gemini parou.

### Hierarquia de Granularidade
1. **Épico** (Gemini CLI): Definição de Produto e Regras de Negócio.
2. **Feature** (Gemini CLI): Decomposição em tarefas técnicas e ADRs.
3. **Tarefa Atômica** (Claude Code ou Codex CLI): Execução técnica.
   - **Regra de Ouro:** Máximo 1 responsabilidade funcional e alteração de no máximo 1 arquivo principal de lógica por tarefa.

### Papéis dos Agentes
| Agente | Papel | Responsabilidade |
|---|---|---|
| Gemini CLI | Arquiteto + Auditor | Planejamento, breakdown, scoring de tarefas críticas (/review), análise de codebase completo, segurança OWASP |
| Claude Code | Engenheiro Sênior | Implementação de tarefas atômicas |
| Codex CLI | Executor Operacional | Implementação terminal-driven, refatoração focada, revisão de diff, validação local e evolução do template |
| Claude.ai | Consultor Estratégico | Contexto, processo, decisões arquiteturais difusas |

### Arquivos de Memória Viva (por projeto — não no template)
- **CONTEXT.md:** Estado atual do projeto (Sprint, decisões críticas, bloqueios). Norte para qualquer IA que entre no projeto.
- **BACKLOG.md:** Lista de Tarefas Atômicas geradas pelo Gemini. Claude Code e Codex CLI consumem daqui.
- **SESSION_LOG.md:** Registro em Português de cada sessão (append). Contém: o que foi feito, o que falhou, próximo passo, branch.

> ⚠️ Esses arquivos existem apenas nos repositórios de produto (FlowOps, CORA).
> O repositório template não possui SESSION_LOG.md — melhorias no template não precisam de rastreamento de sessão.

### Tipos de Sessão
- **Sessão de produto:** entrega feature/bug/melhoria → roda `/task-done` no Claude ou closeout equivalente no Codex (scoring + SAST + confirmação de commit)
- **Sessão de infraestrutura/setup:** entrega arquivos de configuração, templates, comandos → validação manual + Gemini revisa + commit direto

### Regra de Idioma
| Contexto | Idioma |
|---|---|
| Código (variáveis, funções, schemas, commits, nomes de arquivo) | Inglês |
| Instruções para modelos (skills, commands, CLAUDE.md, GEMINI.md) | Inglês |
| Prompt no BACKLOG.md (consumido por Claude Code ou Codex CLI) | Inglês obrigatório |
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
| `.codex/skills/session-start/SKILL.md` | CRIADO ✅ | Início de sessão Codex com seleção de tarefa e confirmação do usuário. |
| `.codex/skills/task-done/SKILL.md` | CRIADO ✅ | Fechamento de tarefa Codex com gates, memória compartilhada e commit confirmado. |
| `.codex/skills/context-load/SKILL.md` | CRIADO ✅ | Componente de carregamento mínimo e ordenado de contexto para Codex. |
| `.codex/skills/atomic-task/SKILL.md` | CRIADO ✅ | Execução incremental de tarefas atômicas pelo Codex. |
| `.codex/skills/review-diff/SKILL.md` | CRIADO ✅ | Revisão focada de diff para bugs, segurança, testes e interoperabilidade. |
| `.codex/skills/task-closeout/SKILL.md` | CRIADO ✅ | Componente de encerramento com validação e memória compartilhada. |
| `TEMPLATE_CODEX.md` | CRIADO ✅ | Contexto de projeto para Codex CLI. |
| `docs/agent-operating-model.md` | CRIADO ✅ | Racional técnico, trade-offs e impactos de interoperabilidade. |
| `.gemini/skills/architecture-critic.md` | CONSOLIDADO ✅ | Integra responsabilidades de health check e pesquisa estratégica. |
| `.gemini/skills/ux-ui-critic.md` | CRIADO ✅ | Foco em consistência de frontend e design system. |
| `.gemini/skills/domain-expert.md` | CRIADO ✅ | Foco em eliciação de requisitos e regras de negócio. |
| `.gemini/skills/qa-engineer.md` | CRIADO ✅ | Foco em estratégia de testes e qualidade. |

---

## 4. Segurança e Privacidade (LGPD por Design)

### Camada de Segurança (SAST)
- **Gate 2 no closeout do executor:** SAST agnóstico de linguagem. Ferramenta configurada no contexto da ferramenta ativa.
- **Exemplos por stack:** Python → `bandit -r ./src` | Node → `npm audit` | TypeScript → `tsc --noEmit`
- **Bloqueio:** vulnerabilidades HIGH ou CRITICAL interrompem o fluxo antes do commit.
- **Commit seguro:** o fluxo de closeout exibe o diff relevante e aguarda confirmação explícita antes de executar commit.

### Estratégia LGPD (por produto)
- **FlowOps:** Base Legal para Execução de Contrato. Regras de exclusão de dados de funcionários ao fim do contrato.
- **CORA:** Dados Sensíveis (Art. 5, IX). Criptografia obrigatória para CPF/CREA, alertas de consentimento para dados de comunidades.
- **Classificação obrigatória por tarefa:** `[PUBLICO | INTERNO | PESSOAL | SENSIVEL]` — campo no BACKLOG.md.

### Próximas implementações (nos projetos, não no template)
- Skill de segurança centralizada (OWASP + multi-tenant) → primeira sessão do FlowOps
- Skill de privacidade LGPD → primeira sessão do CORA

---

### 5. Scoring de Qualidade (Dual)

- **Nível 1 — Auto-avaliação do Executor** (toda tarefa):
  - Cobertura de Testes: peso 3
  - Segurança/Privacidade: peso 4
  - Aderência ao Estilo/DRY: peso 3
  - Score < 7 → gera brief de reinício, bloqueia commit

- **Nível 2 — Auditoria Gemini** (tarefas críticas: Auth, Data Migrations):
  - Gemini CLI faz double-check antes do merge

---

## 6. Estratégia de Arquivamento de Logs (Rolling Archive)

Para evitar que o arquivo `SESSION_LOG.md` cresça indefinidamente e prejudique a janela de contexto das IAs, adotamos a estratégia de **Rolling Archive**:

1.  **Gatilho:** Quando o arquivo atingir ~50 sessões registradas ou ~100KB de tamanho.
2.  **Ação de Arquivamento:**
    *   Renomear o arquivo atual para `memory/session_logs/SESSION_LOG_YYYYMMDD_to_YYYYMMDD.md` (usando as datas da primeira e última sessão do arquivo).
    *   Criar um novo `SESSION_LOG.md` na raiz.
3.  **Preservação de Contexto:**
    *   O novo arquivo deve obrigatoriamente iniciar com o transporte das **últimas 3 sessões** do arquivo anterior.
    *   Isso garante que os fluxos de início de sessão dos executores continuem tendo visibilidade do histórico imediato sem carregar o peso de meses de projeto.
4.  **Execução:** Manual (pelo usuário) ou assistida por comando (futuro), sempre documentando o arquivamento na primeira sessão do novo log.

---

## 7. Decisões Validadas


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
| 12 | Codex CLI entra como executor interoperável, não como substituto de Claude/Gemini | ✅ |

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

## 8. Codex CLI — Modelo Operacional

### Estrutura Codex

- `.codex/skills/session-start`: workflow principal de início, equivalente ao `/session-start` do Claude.
- `.codex/skills/task-done`: workflow principal de fechamento, equivalente ao `/task-done` do Claude.
- `.codex/skills/context-load`: componente que define a ordem mínima de contexto.
- `.codex/skills/atomic-task`: executa uma tarefa pequena com patch incremental.
- `.codex/skills/review-diff`: componente que revisa alterações locais antes de handoff ou commit.
- `.codex/skills/task-closeout`: componente que fecha a sessão com validação e memória compartilhada.
- `CODEX.md`: arquivo instanciado por projeto a partir de `TEMPLATE_CODEX.md`.

### Racional

Codex se beneficia de instruções curtas, operacionais e próximas do fluxo de terminal. Por isso, o template usa duas skills de workflow (`session-start` e `task-done`) que compõem skills menores. Isso preserva o ritual operacional que já funciona no Claude, mas mantém baixo acoplamento e boa reutilização.

### Trade-offs

- Há alguma sobreposição com skills do Claude, mas a fonte de verdade continua sendo compartilhada (`CONTEXT.md`, `BACKLOG.md`, `SESSION_LOG.md`).
- Codex não substitui Gemini em decisões arquiteturais amplas; ele executa e valida mudanças locais.
- O commit continua dependente de confirmação explícita do usuário para preservar segurança operacional.

---

## 9. Próximos Passos

### Template (concluído)
- [x] Sessão 1: templates de memória viva + comandos /session-start e /task-done
- [x] Sessão 2: correções pós-revisão do Gemini
- [x] Sessão 3: camada Codex CLI com skills pequenas e contexto próprio
- [x] Sessão 4: workflows Codex session-start e task-done equivalentes ao fluxo Claude
- [x] Sessão 5: consolidação de skills Gemini e criação de skills especializadas (UX, Domínio, QA)
- [x] Sessão 6: upgrade das diretrizes core do Gemini (Threat Model, Usability Sandbox, Roadmap sync)

### Projetos (a iniciar)
1. Copiar template para FlowOps e CORA
2. Preencher CLAUDE.md, CODEX.md e GEMINI.md de cada projeto com stack real conforme ferramentas usadas
3. Configurar Security Tools no CLAUDE.md de cada projeto
4. Configurar comandos equivalentes em CODEX.md quando Codex for executor ativo
5. FlowOps — Sessão 3: skill de segurança centralizada (OWASP + multi-tenant)
6. CORA — Sessão 3: skill de privacidade LGPD + mapeamento de dados sensíveis

---

## 10. Guia de Instanciação (Checklist)

Este checklist deve ser seguido ao iniciar qualquer novo projeto baseado neste template.

### Fase 1: Setup de Arquivos
- [ ] Criar repositório do projeto.
- [ ] Copiar diretórios `.claude/`, `.codex/` e `.gemini/` conforme ferramentas usadas.
- [ ] Copiar `TEMPLATE_CLAUDE.md` renomeando para `CLAUDE.md`.
- [ ] Copiar `TEMPLATE_CODEX.md` renomeando para `CODEX.md` se o projeto usar Codex CLI.
- [ ] Criar `GEMINI.md` a partir do template (se houver) ou copiar as diretrizes de workflow híbrido.
- [ ] Criar `CONTEXT.md`, `BACKLOG.md` e `SESSION_LOG.md` a partir dos seus respectivos `.template`.

### Fase 2: Configuração do Projeto (`CLAUDE.md`)
- [ ] Definir a **Stack** (Linguagem, Framework, DB, Cloud).
- [ ] Listar os **Comandos** básicos de dev, test e lint.
- [ ] Configurar **Security Tool**:
    - Escolher a ferramenta (ex: `njsscan`, `bandit`, `gosec`).
    - Validar se o comando roda no ambiente atual.
- [ ] Preencher **Security Setup**: `MULTI_TENANT`, `AUTH_METHOD`, etc.
- [ ] Preencher **Privacy Setup**: `LEGAL_BASIS`, `RETENTION`, `PII_FIELDS`.

### Fase 3: Alinhamento Inicial
- [ ] Rodar o comando `/session-start` pela primeira vez para validar se o Claude consegue ler os arquivos de contexto.
- [ ] Rodar a skill `session-start` pela primeira vez para validar se o Codex consegue ler os arquivos de contexto e selecionar a próxima tarefa.
- [ ] Cadastrar a primeira **Tarefa Atômica** na `BACKLOG.md`.
- [ ] Verificar se o `.gitignore` protege segredos (como `.env`).

---

*Documento mantido por Everton. Última atualização: 2026-05-14.*
*Gerado inicialmente pelo Gemini CLI como Arquiteto Sênior.*
