# claude-project-template

Template base de configuração para projetos usando **Claude Code**. Inclui skills de boas práticas de engenharia e comandos prontos para uso em qualquer projeto de software.

---

## Estrutura

```
.claude/
  skills/       # Conhecimento especializado carregado sob demanda
  commands/     # Atalhos de fluxo invocados manualmente
  settings.json # Hooks e permissões
CLAUDE.md       # Contexto do projeto para o Claude
GEMINI.md       # Contexto técnico para o Gemini CLI e chat âncora web
.claudeignore   # Arquivos que o Claude deve ignorar
```

---

## Como usar este template

1. Copie a estrutura para o seu projeto (ou use como GitHub Template Repository)
2. Preencha o `CLAUDE.md` com o stack e convenções do seu projeto
3. As skills serão carregadas automaticamente quando relevantes
4. Os commands são invocados manualmente com `/nome-do-command`

---

## Skills

Skills são instruções especializadas que o Claude carrega dinamicamente. Algumas são ativadas **automaticamente** quando o contexto é relevante — outras você pode invocar diretamente digitando `/nome-da-skill` no Claude Code.

---

### 🏗️ `architecture`
**Quando usar:** Antes de começar qualquer feature nova, ao propor um novo módulo, ou ao avaliar trade-offs de design.

**O que faz:** Guia decisões estruturais antes de codar. Inclui princípios de arquitetura, checklist pré-implementação e formato padrão para ADRs (Architecture Decision Records).

**Como chamar:**
```
/architecture
```
Ou simplesmente pergunte: *"Como devo estruturar esse módulo?"* — o Claude carrega automaticamente.

---

### 💻 `development`
**Quando usar:** Durante o desenvolvimento do dia a dia — ao escrever funções, refatorar código, revisar nomenclatura.

**O que faz:** Define convenções de código, estrutura de funções, padrões de nomenclatura, tratamento de erros e checklist antes de submeter código.

**Como chamar:**
```
/development
```
Ativada automaticamente em tarefas de codificação geral.

---

### 🔒 `security`
**Quando usar:** Em toda e qualquer feature — segurança é transversal. Especialmente em fluxos de autenticação, endpoints de API, manipulação de dados sensíveis e integrações externas.

**O que faz:** Cobre secrets, autenticação, autorização, validação de inputs, segurança de API, auditoria e OWASP Top 10.

**Como chamar:**
```
/security
```
Ativada automaticamente em contextos de autenticação, permissões e dados sensíveis.

---

### 🧪 `qa`
**Quando usar:** Ao escrever testes, revisar cobertura, validar uma feature antes do merge.

**O que faz:** Define a pirâmide de testes, padrões de qualidade, cobertura por tipo de código, testes de contrato de API e checklist pré-merge.

**Como chamar:**
```
/qa
```
Ativada automaticamente quando o contexto envolve testes.

---

### 🗄️ `data`
**Quando usar:** Ao modelar schemas, escrever migrations, otimizar queries, construir pipelines de dados ou lidar com dados sensíveis.

**O que faz:** Padrões de modelagem, regras para migrations, boas práticas de queries, pipelines idempotentes, Medallion Architecture e tratamento de PII.

**Como chamar:**
```
/data
```
Ativada automaticamente ao mencionar banco de dados, migrations, queries ou ETL.

---

### ☁️ `cloud`
**Quando usar:** Ao provisionar recursos, desenhar infraestrutura, revisar políticas IAM ou trabalhar com qualquer serviço cloud. **Toda infraestrutura deve usar Terraform.**

**O que faz:** Padrões universais de IaC com Terraform, tagging obrigatório, IAM, secrets management, observabilidade, e seções específicas para AWS e GCP.

**Como chamar:**
```
/cloud
```
Ativada automaticamente ao mencionar AWS, GCP, Terraform ou infraestrutura.

---

### 🔁 `devops`
**Quando usar:** Ao desenhar pipelines de CI/CD, configurar ambientes, planejar deploys ou trabalhar com Terraform em automação.

**O que faz:** Estratégia de ambientes, estrutura de pipelines CI e CD, Terraform em CI/CD, gestão de secrets em pipelines, estratégia de rollback e branching.

**Como chamar:**
```
/devops
```
Ativada automaticamente em contextos de pipeline, deploy e automação.

---

### 📋 `product`
**Quando usar:** Ao definir requisitos, escrever critérios de aceite, revisar escopo ou fechar uma feature.

**O que faz:** Checklist pré-implementação, formato de critérios de aceite (Given/When/Then), controle de escopo, definição de pronto e padrão de changelog.

**Como chamar:**
```
/product
```
Ativada automaticamente ao discutir requisitos ou escopo de features.

---

### 📝 `documentation`
**Quando usar:** Ao escrever READMEs, ADRs, runbooks, docstrings ou changelogs. Ao finalizar um módulo ou feature.

**O que faz:** Define quais documentos são obrigatórios por projeto, estrutura de README, padrões de documentação de código, uso de diagramas e formato de runbooks.

**Como chamar:**
```
/documentation
```
Ativada automaticamente em tarefas de documentação.

---

### 🔗 `integrations`
**Quando usar:** Ao integrar com APIs externas, implementar OAuth, tratar webhooks ou desenhar lógica de retry/fallback.

**O que faz:** Padrões de autenticação, resiliência (retry, timeout, circuit breaker), idempotência, tratamento de webhooks, contratos de dados e testes de integração.

**Como chamar:**
```
/integrations
```
Ativada automaticamente ao mencionar APIs externas, webhooks ou serviços de terceiros.

---

### 🤖 `gemini`
**Quando usar:** Ao tomar decisões arquiteturais, modelar domínio, realizar análise de segurança ampla, ou sempre que o contexto envolver uma escolha que se beneficia de perspectiva externa antes de implementar. Ativada também ao receber outputs prefixados com `[GEMINI ANALYSIS]` ou `[GEMINI SECURITY]`.

**O que faz:** Define o papel do Gemini Web (chat âncora) e do Gemini CLI no fluxo híbrido. Estabelece gatilhos para sugerir ativamente o uso do Gemini, o que não delegar, como fazer passagem de contexto e quando sincronizar o chat âncora.

**Como chamar:**
```
/gemini
```
Ativada automaticamente em decisões arquiteturais, análise de segurança ampla e ao receber outputs do Gemini.

---

### 🔍 `code-review`
**Quando usar:** Antes de abrir qualquer PR. Invoque para uma revisão sistemática cobrindo todas as dimensões de qualidade.

**O que faz:** Checklist completo: segurança, correção lógica, qualidade de código, testes, dados, cloud/infra, documentação e aspectos gerais.

**Como chamar:**
```
/code-review
```

---

### 💰 `finops`
**Quando usar:** Ao provisionar recursos cloud, revisar mudanças de infraestrutura ou avaliar opções de arquitetura com impacto em custo.

**O que faz:** Estimativa de custo antes de provisionar, alertas de budget, controles de custo para AWS e GCP, boas práticas de Terraform para custo e revisão periódica.

**Como chamar:**
```
/finops
```
Ativada automaticamente ao mencionar custos, billing ou ao aplicar mudanças de infraestrutura em produção.

---

## Commands

Commands são fluxos de trabalho completos que você invoca manualmente. Diferente das skills, eles não são carregados automaticamente — você os chama quando quer executar um processo específico.

---

### ✨ `/new-feature`
**Quando usar:** No início de qualquer feature nova, antes de escrever a primeira linha de código.

**O que faz:** Guia você pelo processo completo de início de feature — clareza de escopo, planejamento de arquivos afetados, identificação de skills relevantes e criação de branch.

```
/new-feature
```

---

### ✅ `/pre-commit`
**Quando usar:** Antes de qualquer commit ou abertura de PR.

**O que faz:** Checklist de qualidade pré-commit — lint, testes, typecheck, audit de dependências e validação manual do diff.

```
/pre-commit
```

---

### 🚀 `/deploy-checklist`
**Quando usar:** Antes de qualquer deploy em staging ou produção.

**O que faz:** Verificação completa pré-deploy — testes, infraestrutura Terraform, migrations, observabilidade e plano de rollback. Inclui checklist pós-deploy para os primeiros 15 minutos.

```
/deploy-checklist
```

---

### 🐛 `/debug`
**Quando usar:** Ao investigar um bug, comportamento inesperado ou incidente em produção.

**O que faz:** Fluxo estruturado de debugging — reprodução, coleta de evidências, hipótese, isolamento, correção e documentação. Evita a armadilha de mudanças aleatórias.

```
/debug
```

---

### 👀 `/review`
**Quando usar:** Antes de abrir um PR. Versão acionável do `code-review` skill.

**O que faz:** Executa a skill de code-review contra o diff atual ou arquivo especificado, retornando um checklist com ✅ / ❌ / ⚠️ e um resumo de bloqueadores.

```
/review
/review src/module/arquivo.py
```

---

### 🔭 `/gemini-analyze`
**Quando usar:** Ao querer uma visão global do codebase — inconsistências arquiteturais, débito técnico, violações de convenção — sem precisar chunkar arquivos manualmente.

**O que faz:** Invoca o Gemini CLI com a janela de 1M tokens para analisar o repositório inteiro ou um módulo específico. Retorna findings estruturados sobre arquitetura, débito técnico e inconsistências, prefixados com `[GEMINI ANALYSIS]`.

```
/gemini-analyze
/gemini-analyze src/payments
```

---

### 🔐 `/gemini-security`
**Quando usar:** Antes de abrir um PR com mudanças sensíveis, ou ao revisar um módulo crítico de segurança.

**O que faz:** Invoca o Gemini CLI para revisão de segurança focada em OWASP Top 10, multi-tenancy e secrets. Cobre o diff atual ou um módulo específico. Findings retornam com severidade (CRITICAL / HIGH / MEDIUM / LOW) e recomendação de fix, prefixados com `[GEMINI SECURITY]`.

```
/gemini-security
/gemini-security src/auth
```

---

### 🔄 `/gemini-sync`
**Quando usar:** Após decisões arquiteturais significativas, adição de novos módulos, ou atualização do `GEMINI.md`.

**O que faz:** Gera um bloco estruturado de atualização para colar no chat âncora do Gemini Web, mantendo o arquiteto externo sincronizado com o estado atual do projeto. Inclui mudanças recentes, ADRs aceitos e contexto para próximas discussões.

```
/gemini-sync
```

---

## Customizando para seu projeto

Ao usar este template em um novo projeto, preencha o `CLAUDE.md` com:

- **Stack** — linguagens, frameworks, banco de dados, cloud
- **Commands** — comandos reais de dev, test, build e lint do projeto
- **Conventions** — padrões específicos do time
- **Do Not Touch** — arquivos críticos que o Claude não deve modificar
- **Known Errors** — erros recorrentes que já foram identificados e como evitá-los

As skills não precisam de modificação para a maioria dos projetos — são intencionalmente genéricas.

---

## Referências

- [anthropics/skills](https://github.com/anthropics/skills) — Repositório oficial de Agent Skills da Anthropic
- [Claude Code Docs — Skills](https://code.claude.com/docs/en/skills) — Documentação oficial
- [agentskills.io](https://agentskills.io) — Especificação aberta do padrão Agent Skills