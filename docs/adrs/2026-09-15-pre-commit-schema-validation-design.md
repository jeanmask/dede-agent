# ADR 005: Pre-commit Centralizado em JS (Husky)

**Status:** Aceito
**Data:** 2026-09-15

## Contexto e Problema

A validação atual de YAML e do JSON Schema era realizada estritamente durante o fluxo de Continuous Integration (CI) utilizando as Actions `action-yamllint` e `ajv-cli`.
Isso ocasionava loops de feedback lentos caso o desenvolvedor cometesse erros de sintaxe ou de formatação no arquivo de configuração, descobrindo o problema apenas após um push. Além disso, as ferramentas eram heterogêneas (Python para `yamllint`, Node para `ajv-cli` e `markdownlint`).

## Decisão

Foi decidido padronizar todas as validações de código e formatação utilizando o ecossistema JavaScript/Node.js, adotando o **Husky** com **lint-staged**.
Isso elimina a necessidade de instalar Python e o framework `pre-commit` localmente.

Adotamos a seguinte arquitetura no `package.json`:

1. **Husky & lint-staged:** Os hooks de git disparam apenas nos arquivos atualmente sendo commitados (staged), deixando o pre-commit rápido.
2. **Prettier:** Substituiu o `yamllint` (Python). Ele garante auto-formatação sem falsos positivos rigorosos (como limites estritos de linha em YAMLs descritivos).
3. **AJV-CLI (Schema Validation):** Configurado para validar apenas o `templates/config.yaml` contra o schema JSON.
4. **Markdown Lint (`markdownlint-cli2`):** Integrado como auto-fix para padronizar os documentos.

## Consequências

- **Positivas:** Um único sistema de dependências (`npm`). Auto-fix aplicado automaticamente em arquivos `.md` e `.yaml` no momento do commit via `lint-staged`. CI perfeitamente em paridade com o ambiente local.
- **Negativas:** Requer instalação do Node.js (algo já onipresente no ecossistema moderno) e `npm install` após clonar.
