# ADR 005: Pre-commit e Validação YAML / Schema

**Status:** Aceito
**Data:** 2026-09-15

## Contexto e Problema

A validação atual de YAML e do JSON Schema era realizada estritamente durante o fluxo de Continuous Integration (CI) utilizando as Actions `action-yamllint` e `ajv-cli`.
Isso ocasionava loops de feedback lentos caso o desenvolvedor cometesse erros de sintaxe ou de formatação no arquivo de configuração, descobrindo o problema apenas após um push para o repositório remoto.

## Decisão

Foi decidido implementar a validação local por meio da adoção do `pre-commit`.
O `pre-commit` é o padrão da indústria para a gestão de git hooks locais, permitindo a execução rápida e segura das validações antes da criação dos commits.

Adotamos a seguinte arquitetura de validação no `.pre-commit-config.yaml`:

1. **Yamllint:** Utilização do hook oficial do repositório do `yamllint`. Para evitar burocracia e conflitos com textos e links longos em Markdown/YAML, utilizamos um `.yamllint.yaml` customizado relaxando `line-length` e desativando a exigência de `document-start`.
2. **AJV-CLI (Schema Validation):** Para manter paridade total (1:1) com o fluxo de CI, configuramos um hook `local` executando diretamente `npx ajv-cli validate`. Isso assegura o uso da mesma engine do CI sem adicionar dependências incompatíveis ou hooks paralelos que validadariam os schemas de forma distinta.
3. **Markdown Lint (`markdownlint-cli2`):** Adicionado um hook `local` executando diretamente `npx markdownlint-cli2` para validar formatações de documentação, refletindo a CI e utilizando os arquivos de configuração `.markdownlint.json`.

## Consequências

- **Positivas:** Feedback instantâneo para desenvolvedores ao modificar configurações (como `templates/config.yaml`), evitando commits de regras quebradas.
- **Negativas:** Exige que todos os contribuidores tenham o ambiente minimamente configurado (Node.js e Python) e executem `pre-commit install` localmente para usufruir da validação.
