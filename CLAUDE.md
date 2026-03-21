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

O usuário já validou o output antes de trazer — não questionar a fonte, implementar a partir do contexto recebido.

### Commands disponíveis

- `/gemini-analyze` — análise global ou por módulo via Gemini CLI
- `/gemini-security` — revisão de segurança do diff ou módulo
- `/gemini-sync` — gera bloco de atualização para o chat âncora

## Known Errors
<!-- Format: ### Error title / Cause / Solution -->
