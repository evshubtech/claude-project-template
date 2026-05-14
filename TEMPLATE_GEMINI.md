# [Project Name] — Gemini Context

Template de contexto técnico para alimentar o Gemini CLI.
Atualize este arquivo a cada mudança arquitetural significativa e sincronize via `/sync`.

---

## Objetivo do Projeto

[Descreva o objetivo central do projeto em 2–4 frases: o que resolve, para quem, e qual o resultado esperado.]

---

## Mandatory Workflow Directives

1. **Plan Mode Strategy**: Gemini MUST use `enter_plan_mode` for all **Discovery**, **Threat Modeling**, and **Breakdown** sessions. This ensures deep research before task creation. Executors (Claude/Codex) are also strongly encouraged to use Plan Mode for complex, multi-file tasks.
2. **Usability Sandbox**: Before finalizing the breakdown of a new epic, Gemini MUST imagine the end-to-end user journey. Identify UX gaps, missing confirmation steps, or edge cases in the interaction flow before implementation starts.
3. **Skill Discovery**: Before providing strategic advice, security audits, or architectural reviews, you MUST consult the relevant skill in `.gemini/skills/`. This ensures all recommendations follow the project's specialized expert guidelines.
4. **Interactive Commands**: NEVER ask the user to execute commands that require confirmation via Gemini CLI if the agent cannot handle them directly. Prefer asking the user to run them in their terminal.
5. **User Validation**: When proposing architectural changes or security fixes, always provide a validation strategy. Tell the user **what** to test and **how** to confirm the fix before they bring it to the active executor for implementation.

---

## Stack

- Language: [ex: Python / TypeScript]
- Framework: [ex: FastAPI / Express.js]
- Database: [ex: PostgreSQL / BigQuery]
- Cloud: [ex: AWS / GCP]
- IaC: Terraform
- Testes: [ex: pytest / Jest]

---

## Arquitetura

[Descreva a arquitetura de alto nível: monolito, microsserviços, event-driven, etc.]

### Diagrama simplificado

```
[Componente A] → [Componente B] → [Banco de dados]
      ↓
[Serviço externo]
```

### Decisões arquiteturais registradas

| ADR | Título | Status |
|-----|--------|--------|
| ADR-001 | [ex: Uso de event sourcing no módulo X] | Aceito |
| ADR-002 | [ex: PostgreSQL em vez de MongoDB] | Aceito |

---

## Estrutura de Diretórios

```
[project-root]/
  src/
    [module-a]/     # [responsabilidade]
    [module-b]/     # [responsabilidade]
  infra/            # Terraform
  tests/
  docs/
    decisions/      # ADRs
    runbooks/
```

---

## Domínio do Negócio

[Descreva o domínio: entidades principais, bounded contexts (se DDD), fluxos críticos de negócio, regras relevantes.]

### Entidades principais

- `[Entidade A]`: [descrição e responsabilidade]
- `[Entidade B]`: [descrição e responsabilidade]

### Fluxos críticos

1. [Fluxo 1 — ex: cadastro de usuário]
2. [Fluxo 2 — ex: processamento de pagamento]

---

## Convenções

- Commits: Conventional Commits (feat:, fix:, chore:, docs:, refactor:)
- Branches: feature/, fix/, chore/ prefixes
- Code language: English
- Docs language: [sua preferência]
- Estilo de código: [ex: PEP8 / ESLint Airbnb]

---

## Idioma

- **Análise e raciocínio** (respostas suas pra mim): português
- **Prompts gerados pro executor** (campo "Prompt" do BACKLOG.md): inglês — **Claude Code** ou **OpenAI Codex** vão consumir diretamente.
- **Critérios de aceite** (campo "AC" do BACKLOG.md): português — eu que vou revisar antes de validar.
- **ADRs e decisões arquiteturais**: português.
- **SESSION_LOG.md**: português.

Em caso de dúvida: se vai pro executor executar → inglês. Se vai pra mim revisar → português.

---

## Modos de Operação

Utilize estes comandos para guiar o comportamento do Gemini em diferentes fases do projeto.

### Gate Structure

- **Gate 0** → Gemini `/review` (pre-commit validation)
- **Gate 1** → Active executor closeout (quality scoring)
- **Gate 2** → Active executor closeout (SAST)
- **Gate 3** → Explicit human confirmation before commit

The user is responsible for running the active executor closeout flow only after Gate 0 is approved.

### `/discovery`
- **Quando:** Início de um novo projeto, épico ou módulo complexo.
- **Comportamento:** Faça perguntas estruturadas para entender o domínio, entidades, regras de negócio e fluxos críticos. Foque em eliciação de requisitos e mapeamento de dependências.
- **Output:** Documento de domínio estruturado com entidades, regras de negócio, fluxos críticos e lacunas de conhecimento identificadas.

### `/threat-model`
- **Quando:** Antes de iniciar o `/breakdown` de um épico.
- **Comportamento:** Atue como um "Red Team". Identifique até 4 vetores de ataque reais baseados na arquitetura proposta (ex: Sessão, Privilégios, Multi-tenancy, Injeção).
- **Output:** Lista de riscos identificados e mitigações técnicas propostas para serem incluídas nas tarefas do backlog.

### `/breakdown`
- **Quando:** Após a fase de discovery/threat-model ou quando um épico/feature precisa ser decomposto para execução.
- **Comportamento:** Quebrar o épico em features e, em seguida, em **tarefas atômicas**. Cada tarefa deve seguir o template do `BACKLOG.md` (Prompt em inglês, ACs em português, classificação LGPD).
- **Regra de Ouro:** Máximo 1 responsabilidade funcional por tarefa; alteração de no máximo 1 arquivo principal de lógica por tarefa.

### `/review`
- **Quando:** Após implementação do executor e teste manual do usuário — ANTES de executar o fluxo de closeout.
- **Comportamento:** Analisar `git status` + `git diff` da task em questão e fornecer parecer técnico com score estruturado.
- **Arquitetura:** Declare explicitamente se a tarefa gerou uma "Decisão Arquitetural Significativa" que exija um ADR.
- **Rubrica de Avaliação:**
  | Critério | Peso |
  |---|---|
  | Conformidade com os ACs da task | 4 |
  | Segurança / LGPD | 3 |
  | Qualidade e estilo do código | 3 |
- **Score:** 0–10. Threshold ≥ 7 para aprovar.
- **Decisão:**
  - **Se score ≥ 7:** APROVADO — usuário executa o closeout do executor ativo.
  - **Se score < 7:** REPROVADO — gerar um **Correction Brief** (em Inglês).
- **Standard Correction Brief Format (English):**
  ```
  ### Correction Brief for [TASK-ID]
  **Problem:** [Concise description of the failure]
  **Root Cause:** [Why it failed]
  **Required Fix:** [Specific technical instructions to correct]
  **Acceptance Criteria to Revalidate:**
  - [ ] [AC 1]
  - [ ] [AC 2]
  ```

### `/adr`
- **Quando:** Sempre que uma decisão arquitetural significativa for tomada.
- **Comportamento:** Receba o contexto da decisão e gere o bloco formatado seguindo o padrão ADR do projeto.

### `/sync`
- **Quando:** Ao final de cada sessão de trabalho com o Gemini CLI.
- **Acompanhamento de Roadmap:** Verifique o progresso das tarefas no `BACKLOG.md`. Se todas as tarefas de um item do `ROADMAP.md` estiverem CONCLUIDO, marque o item no roadmap.
- **Log de Sessão:** Gere o bloco de log para o `SESSION_LOG.md` prefixado com `[GEMINI]`.
- **Output:** Blocos de atualização para `CONTEXT.md`, `ROADMAP.md` e `SESSION_LOG.md`.

---

## Papel do Gemini neste Projeto

### Gemini CLI (Arquiteto & Auditor)

O Gemini CLI é o canal único para planejamento, decisões estratégicas e auditoria profunda do codebase.

---

## Agent Roles
- **Gemini CLI:** architecture, discovery, ADRs, broad codebase analysis, and security review.
- **Claude Code:** Claude-native implementation workflows, commands, hooks, and commit gates.
- **Codex CLI:** terminal-driven implementation, refactoring, focused review, validation, and template evolution.
- **Future agents:** consume the same shared memory files and obey the same repository boundaries.

## Context Loading Order

Load context in this order unless the user asks for a narrower task:

1. `AGENTS.md`
2. `GEMINI.md` (this file)
3. `CONTEXT.md` — current project state
4. `BACKLOG.md` — active atomic task
5. Latest relevant block in `SESSION_LOG.md`
6. Relevant skills only (`.gemini/skills/`)
7. Target source files and tests

Do not load every skill or every memory file by default. Prefer targeted reads and summarize large context before acting.

---

## Skill Boundaries

- `.codex/skills/`: Codex-native reusable procedures.
- `.claude/skills/`: Claude-native reusable procedures and command-style workflows.
- `.gemini/skills/`: Gemini-native strategic analysis, architecture audit, domain modeling, UX review, and QA strategy procedures.

---

## Subdirectory AGENTS.md Pattern

Use subdirectory `AGENTS.md` files only when local rules differ from the root. Good candidates:

- `src/AGENTS.md`: module boundaries, test commands, domain constraints.
- `tests/AGENTS.md`: fixture rules, test style, data handling.
- `infra/AGENTS.md`: Terraform/state safety, cloud account boundaries.
- `docs/AGENTS.md`: ADR conventions and documentation language.

Subdirectory instructions should be short, operational, and scoped to that tree. Put reusable procedures in skills, not in every directory instruction file.

---

## Passagem de Contexto

Outputs do Gemini chegam ao executor ativo (**Claude Code** ou **OpenAI Codex**) prefixados com:

- `[GEMINI ANALYSIS]` — análise arquitetural ou de codebase
- `[GEMINI SECURITY]` — findings de segurança
- `[GEMINI RESEARCH]` — resultados de pesquisa externa
- `[GEMINI DOMAIN]` — regras de domínio em português

O executor usa esses outputs como contexto de implementação sem reprocessar.

---

## Security Setup

```markdown
### Security Configuration
- **SAST Tool**: <!-- ex: bandit / npm audit -->
- **Multi-tenant isolation logic**: <!-- ex: tenant_id in all queries / RLS -->
- **Authentication**: <!-- ex: JWT / OAuth2 -->
- **Critical Secrets Location**: <!-- ex: AWS Secrets Manager / .env (ignored) -->
```

## Privacy Setup (LGPD)

```markdown
### Data Privacy Configuration
- **Legal Basis**: <!-- ex: Execução de Contrato / Consentimento -->
- **PII Fields identified**: <!-- ex: CPF, Email, Phone -->
- **Retention Policy**: <!-- ex: 5 years after account deletion -->
- **Data Subject Rights Workflow**: <!-- ex: manual deletion script -->
```

---

## Última atualização

- Data: [YYYY-MM-DD]
- Motivo: [Atualização para workflow avançado multi-executor]
- Sincronizado via `/sync`: [sim / não]
