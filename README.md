# claude-project-template

Template base de configuração para projetos de alto nível usando **Claude Code**, **Gemini CLI** e **Codex CLI**. Inclui um ecossistema completo de skills, automação de fluxo de trabalho (Gates), contexto compartilhado e conformidade LGPD por design.

---

## Estrutura do Repositório

```
.claude/
  skills/          # Skills e comandos unificados (Arquitetura, Fluxo, IA)
  settings.json    # Configurações e hooks do Claude Code
.codex/
  skills/          # Skills pequenas para contexto, execução, revisão e closeout
.gemini/
  skills/          # Skills para o Gemini CLI (Auditoria global, ADR, Discovery)
AGENTS.md          # Entrada model-agnostic para todos os agentes
CLAUDE.md          # Contexto do projeto para o Claude (Stack, Setup, Convenções)
CODEX.md           # Contexto do projeto para Codex CLI (execução e validação)
GEMINI.md          # Contexto técnico para o Gemini CLI (Janela de 1M tokens)
PLANO_DIRETOR.md   # Visão estratégica e regras do ecossistema de agentes
BACKLOG.md.template     # Template para tarefas atômicas
CONTEXT.md.template     # Template para o estado vivo do projeto
SESSION_LOG.md.template # Template para registro histórico das sessões
```

---

## Fluxo de Trabalho (Multiagente)

O projeto utiliza um modelo de agentes especializados para garantir integridade arquitetural, execução técnica rigorosa e continuidade entre sessões.

### 1. Gemini CLI (Arquiteto & Auditor)
O Gemini atua na camada de estratégia e descoberta, utilizando sua janela de contexto massiva (1M+ tokens).
- **Fluxo:** `/discovery` → `/threat-model` → `/adr` → `/sync` → `/breakdown` (gera BACKLOG.md) → `/review`.
- **Papel:** Define a arquitetura, realiza modelagem de ameaças, toma decisões complexas e audita o codebase globalmente com skills especializadas (UX, Domínio, QA).

### 2. Claude Code (Engenheiro Sênior)
O Claude atua na camada de execução de tarefas atômicas e entrega técnica.
- **Fluxo:** `/session-start` → Implementação → `/task-done`.
- **Papel:** Escreve código, roda testes, corrige bugs e realiza commits seguros.

### 3. Codex CLI (Executor Operacional)
O Codex atua na camada de implementação terminal-driven, refatoração focada, revisão de diff e evolução do template.
- **Fluxo:** `session-start` → `atomic-task` → Gemini `/review` → `task-done`.
- **Papel:** Inspeciona o repositório, edita arquivos de forma incremental, roda comandos locais, preserva mudanças existentes e mantém interoperabilidade com Claude/Gemini.

---

## Gates de Qualidade

Toda alteração de código deve passar por quatro gates obrigatórios antes do commit:

- **Gate 0 (Gemini Review):** Auditoria arquitetural em mudanças críticas.
- **Gate 1 (Executor Scoring):** Auto-avaliação Claude/Codex (Score >= 7) baseada em testes e aderência.
- **Gate 2 (SAST):** Bloqueio obrigatório em vulnerabilidades HIGH/CRITICAL.
- **Gate 3 (Commit Humano):** Exibição do diff e confirmação explícita antes de commit.

---

## Arquivos de Memória Viva

Estes arquivos gerenciam o contexto entre sessões e agentes (existem apenas nos projetos instanciados):

- **`CONTEXT.md`**: O "Norte" do projeto. Contém o estado atual da sprint, decisões críticas e bloqueios.
- **`BACKLOG.md`**: Lista de **Tarefas Atômicas** (máximo 1 responsabilidade por tarefa) com critérios de aceite e classificação LGPD.
- **`SESSION_LOG.md`**: Registro histórico (em Português) de cada sessão de trabalho, mantendo a continuidade do raciocínio. Prefixos válidos: `[CLAUDE]`, `[CODEX]`, `[GEMINI]`.

---

## Como usar este template (Instanciação)

Siga o checklist do `PLANO_DIRETOR.md` para iniciar um novo projeto:

### Fase 1: Setup de Arquivos
- [ ] Criar repositório do projeto.
- [ ] Copiar `.claude/` e `.gemini/` para a raiz do novo projeto.
- [ ] Copiar `.codex/` se o projeto usar Codex CLI.
- [ ] Criar `CLAUDE.md`, `CODEX.md` e `GEMINI.md` a partir dos templates necessários.
- [ ] Criar `CONTEXT.md`, `BACKLOG.md` e `SESSION_LOG.md` usando os arquivos `.template`.

### Fase 2: Configuração
- [ ] Definir a **Stack** e os **Comandos** reais (dev, test, lint).
- [ ] Configurar a **Security Tool** (ex: `bandit`, `njsscan`, `npm audit`).
- [ ] Definir regras de **Privacidade** (PII Fields, Legal Basis).

### Fase 3: Alinhamento
- [ ] Rodar `/session-start` para validar a leitura do contexto.
- [ ] Rodar `session-start` se Codex CLI estiver habilitado.
- [ ] Cadastrar a primeira tarefa atômica no `BACKLOG.md`.

---

## Skills de Workflow (Comandos)

As skills são carregadas automaticamente pelo contexto ou invocadas conforme o padrão da ferramenta. As principais skills de fluxo são:

- **`/session-start`**: Inicia a sessão injetando contexto e histórico.
- **`/task-done`**: Finaliza a tarefa disparando os Gates de qualidade e commit.
- **`/sync`**: Sincroniza decisões de arquitetura e progresso no `CONTEXT.md`.
- **`/new-feature`**: Guia o planejamento inicial de uma nova funcionalidade.
- **`/review`**: Executa uma auditoria de código detalhada contra as skills do projeto.
- **`/gemini-analyze` / `/gemini-security`**: Invocam o Gemini CLI para análises profundas.

### Codex Skills

- **`session-start`**: Inicia a sessão Codex lendo memória compartilhada, escolhendo a próxima tarefa `PRONTO` e confirmando escopo com o usuário.
- **`task-done`**: Fecha a tarefa Codex após Gate 0 do Gemini, roda scoring, segurança, revisão de diff, memória e commit com confirmação explícita.
- **`context-load`**: Carrega o mínimo contexto útil na ordem correta.
- **`atomic-task`**: Executa uma tarefa pequena com edição incremental e validação.
- **`review-diff`**: Revisa o diff atual com foco em bugs, segurança, testes e interoperabilidade.
- **`task-closeout`**: Componente reutilizável para evidências de validação e atualização de memória quando aplicável.

---

## Referências

- [Claude Code Docs](https://code.claude.com/docs)
- [Gemini CLI Documentation](https://github.com/google/gemini-cli)
- [LGPD - Lei Geral de Proteção de Dados](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709.htm)
