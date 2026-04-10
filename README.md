# claude-project-template

Template base de configuração para projetos de alto nível usando **Claude Code** e **Gemini CLI**. Inclui um ecossistema completo de skills, automação de fluxo de trabalho (Gates) e conformidade LGPD por design.

---

## Estrutura do Repositório

```
.claude/
  skills/          # Skills e comandos unificados (Arquitetura, Fluxo, IA)
  settings.json    # Configurações e hooks do Claude Code
.gemini/
  skills/          # Skills para o Gemini CLI (Auditoria global, ADR, Discovery)
CLAUDE.md          # Contexto do projeto para o Claude (Stack, Setup, Convenções)
GEMINI.md          # Contexto técnico para o Gemini CLI (Janela de 1M tokens)
PLANO_DIRETOR.md   # Visão estratégica e regras do ecossistema de agentes
BACKLOG.md.template     # Template para tarefas atômicas
CONTEXT.md.template     # Template para o estado vivo do projeto
SESSION_LOG.md.template # Template para registro histórico das sessões
```

---

## Fluxo de Trabalho (Híbrido)

O projeto utiliza um modelo de dois agentes especializados para garantir a integridade arquitetural e a execução técnica rigorosa.

### 1. Gemini CLI (Arquiteto & Auditor)
O Gemini atua na camada de estratégia e descoberta, utilizando sua janela de contexto massiva (1M+ tokens).
- **Fluxo:** `/discovery` → `/adr` → `/sync` → `/breakdown` (gera BACKLOG.md) → `/review`.
- **Papel:** Define a arquitetura, toma decisões complexas e audita o codebase globalmente.

### 2. Claude Code (Engenheiro Sênior)
O Claude atua na camada de execução de tarefas atômicas e entrega técnica.
- **Fluxo:** `/session-start` → Implementação → `/task-done`.
- **Papel:** Escreve código, roda testes, corrige bugs e realiza commits seguros.

---

## Gates de Qualidade

Toda alteração de código deve passar por quatro gates obrigatórios antes do commit:

- **Gate 0 (Gemini Review):** Auditoria arquitetural em mudanças críticas.
- **Gate 1 (Claude Scoring):** Auto-avaliação (Score >= 7) baseada em testes e aderência.
- **Gate 2 (Claude SAST):** Bloqueio obrigatório em vulnerabilidades HIGH/CRITICAL.
- **Gate 3 (Claude Commit):** Exibição do diff e confirmação explícita humana.

---

## Arquivos de Memória Viva

Estes arquivos gerenciam o contexto entre sessões e agentes (existem apenas nos projetos instanciados):

- **`CONTEXT.md`**: O "Norte" do projeto. Contém o estado atual da sprint, decisões críticas e bloqueios.
- **`BACKLOG.md`**: Lista de **Tarefas Atômicas** (máximo 1 responsabilidade por tarefa) com critérios de aceite e classificação LGPD.
- **`SESSION_LOG.md`**: Registro histórico (em Português) de cada sessão de trabalho, mantendo a continuidade do raciocínio.

---

## Como usar este template (Instanciação)

Siga o checklist do `PLANO_DIRETOR.md` para iniciar um novo projeto:

### Fase 1: Setup de Arquivos
- [ ] Copiar `.claude/` e `.gemini/` para a raiz do novo projeto.
- [ ] Criar `CLAUDE.md` e `GEMINI.md` a partir dos templates.
- [ ] Criar `CONTEXT.md`, `BACKLOG.md` e `SESSION_LOG.md` usando os arquivos `.template`.

### Fase 2: Configuração (`CLAUDE.md`)
- [ ] Definir a **Stack** e os **Comandos** reais (dev, test, lint).
- [ ] Configurar a **Security Tool** (ex: `bandit`, `njsscan`, `npm audit`).
- [ ] Definir regras de **Privacidade** (PII Fields, Legal Basis).

### Fase 3: Alinhamento
- [ ] Rodar `/session-start` para validar a leitura do contexto.
- [ ] Cadastrar a primeira tarefa atômica no `BACKLOG.md`.

---

## Skills de Workflow (Comandos)

As skills são carregadas automaticamente pelo contexto ou invocadas com `/nome-da-skill`. As principais skills de fluxo são:

- **`/session-start`**: Inicia a sessão injetando contexto e histórico.
- **`/task-done`**: Finaliza a tarefa disparando os Gates de qualidade e commit.
- **`/sync`**: Sincroniza decisões de arquitetura e progresso no `CONTEXT.md`.
- **`/new-feature`**: Guia o planejamento inicial de uma nova funcionalidade.
- **`/review`**: Executa uma auditoria de código detalhada contra as skills do projeto.
- **`/gemini-analyze` / `/gemini-security`**: Invocam o Gemini CLI para análises profundas.

---

## Referências

- [Claude Code Docs](https://code.claude.com/docs)
- [Gemini CLI Documentation](https://github.com/google/gemini-cli)
- [LGPD - Lei Geral de Proteção de Dados](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709.htm)
