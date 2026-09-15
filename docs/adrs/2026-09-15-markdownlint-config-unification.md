# ADR: Unificação da Configuração do Markdownlint

## 1. Contexto

Inicialmente, o projeto possuía o arquivo `.markdownlint.json` para relaxamento de regras (ex: `MD013` e `MD060`). Para resolver um problema do CI com arquivos em `node_modules`, introduzimos o arquivo `.markdownlint-cli2.jsonc` apenas com a propriedade `"ignores"`.

Isso resultou na existência de dois arquivos JSON na raiz para lidar com configurações da mesma ferramenta, gerando ruído e possível confusão sobre onde adicionar futuras configurações.

## 2. Decisão

Decidimos unificar todas as configurações do markdownlint em um único arquivo: `.markdownlint-cli2.jsonc`.

A propriedade `"config"` suportada nativamente pelo `markdownlint-cli2` foi utilizada para abrigar as regras de lint (antes no `.markdownlint.json`), enquanto a propriedade `"ignores"` continua sendo responsável pelas exclusões de pastas.

O arquivo `.markdownlint.json` localizado na raiz foi excluído, e as diretrizes em `rules/AGENTS.md` foram atualizadas para apontar a nova fonte de verdade.

## 3. Consequências

- Redução de arquivos de configuração na raiz do projeto.
- Centralização das configurações do linter, facilitando a manutenção.
- O arquivo `.markdownlint.json` localizado na pasta `templates/` (se existir) deve ser mantido caso possua regras específicas isoladas para a geração de templates, mas a raiz agora usa estritamente o formato JSONC do CLI 2.
