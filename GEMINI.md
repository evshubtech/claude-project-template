# [Project Name] — Gemini Context

Template de contexto técnico para alimentar o Gemini CLI e o chat âncora web.
Atualize este arquivo a cada mudança arquitetural significativa e sincronize com o chat âncora via `/gemini-sync`.

---

## Objetivo do Projeto

[Descreva o objetivo central do projeto em 2–4 frases: o que resolve, para quem, e qual o resultado esperado.]

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

## Papel do Gemini neste Projeto

### Gemini Web (chat âncora)

Use o chat âncora para decisões **antes** de implementar:

- Análise de domínio e modelagem DDD
- Decisões arquiteturais e trade-offs
- Compliance e raciocínio regulatório
- Pesquisa com Google Search grounding
- Análise de PDFs técnicos e diagramas de arquitetura
- Brainstorming e exploração de soluções alternativas

### Gemini CLI (terminal)

Use o Gemini CLI para análise **do codebase** sem necessidade de chunkar:

- Análise global do repositório com janela de 1M tokens
- Revisão de segurança (OWASP, multi-tenancy, secrets)
- Mapeamento de débito técnico e inconsistências arquiteturais
- Validação de conformidade com as convenções do projeto

---

## O que NÃO delegar ao Gemini

- Geração de código que vai direto ao repositório
- Decisões de segurança críticas de implementação
- Correções de bugs pontuais e refatorações locais
- Testes unitários e de integração
- Git flow e commits

---

## Passagem de Contexto

Outputs do Gemini chegam ao Claude Code prefixados com:

- `[GEMINI ANALYSIS]` — análise arquitetural ou de codebase
- `[GEMINI SECURITY]` — findings de segurança

O Claude Code usa esses outputs como contexto de implementação sem reprocessar — o usuário já validou antes de trazer.

---

## Última atualização

- Data: [YYYY-MM-DD]
- Motivo: [ex: adição do módulo de pagamentos / decisão arquitetural X]
- Sincronizado com chat âncora: [sim / não]
