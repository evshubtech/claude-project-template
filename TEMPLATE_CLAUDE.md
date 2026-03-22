# [Project Name]

## Stack
- Language: [ex: Python / TypeScript]
- Framework: [ex: FastAPI / Express.js]
- Database: [ex: PostgreSQL / BigQuery]
- Cloud: [ex: AWS / GCP]
- IaC: Terraform

## Commands
- `[dev command]`
- `[test command]`
- `[build command]`
- `[lint command]`

## Mandatory Workflow Directives
1. **Skill Discovery**: Before starting any non-trivial task, you MUST check the `.claude/skills/` directory and load the relevant expert skill(s). A task is "non-trivial" if it involves architecture, security, data modeling, cloud infra, or complex business logic.
2. **Interactive Commands**: NEVER execute commands that require terminal confirmation or user input (e.g., `db:push`, `terraform apply`, interactive migrations). If a command requires confirmation, provide the exact command and ask the user to run it.
3. **User Validation**: BEFORE every commit or PR, you MUST ask the user to manually test and validate the changes. Provide clear instructions on **what** to verify and **how** to verify (including specific paths, expected outputs, or test scripts).
4. **Commit Protocol**: A task is only complete AFTER user validation. Do not assume success or commit without explicit confirmation that the user has verified the behavior.

## Conventions
- Commits: Conventional Commits (feat:, fix:, chore:, docs:, refactor:)
- Branches: feature/, fix/, chore/ prefixes
- Code language: English
- Docs language: [your preference]

## Do Not Touch
- Migration files already applied
- [other critical files/folders]

## Hybrid Workflow — Gemini

Este projeto usa um fluxo híbrido entre Claude Code e Gemini. Consulte `GEMINI.md` para o contexto técnico completo do projeto.

### Papel do Gemini Web (chat âncora)

Consultar **antes** de implementar:
- Decisões arquiteturais e trade-offs
- Modelagem de domínio e DDD
- Compliance e raciocínio regulatório
- Análise de PDFs técnicos e diagramas
- Brainstorming de soluções alternativas

### Papel do Gemini CLI (terminal)

Invocar para análise **do codebase** com janela de 1M tokens:
- Análise global do repositório sem chunkar
- Revisão de segurança (OWASP, multi-tenancy, secrets)
- Mapeamento de débito técnico e inconsistências

### Quando sugerir ativamente consultar o Gemini

Sugerir Gemini Web quando o usuário:
- Pergunta como estruturar/modelar algo não trivial
- Levanta questões de compliance ou regulação
- Está prestes a tomar uma decisão arquitetural que afeta múltiplos módulos

Sugerir Gemini CLI quando:
- O usuário quer revisão de segurança ampla do codebase
- Há perguntas sobre consistência de padrões entre muitos arquivos

### Como tratar outputs prefixados

- `[GEMINI ANALYSIS]` — análise arquitetural ou de codebase: usar diretamente como contexto de implementação, sem reprocessar
- `[GEMINI SECURITY]` — findings de segurança: tratar por severidade (CRITICAL bloqueia merge, HIGH bloqueia PR)
- `[GEMINI RESEARCH]` — fatos externos e documentação: seguir diretrizes de API/Docs pesquisadas pelo Gemini
- `[GEMINI DOMAIN]` — regras de negócio: seguir lógica de domínio em Português

O usuário já validou o output antes de trazer — não questionar a fonte, implementar a partir do contexto recebido.

### Commands disponíveis

- `/gemini-analyze` — análise global ou por módulo via Gemini CLI
- `/gemini-security` — revisão de segurança do diff ou módulo
- `/agents-sync` — gera bloco de atualização para os chats âncora

## Known Errors
<!-- Format: ### Error title / Cause / Solution -->
