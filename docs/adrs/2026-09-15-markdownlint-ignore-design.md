# ADR: Markdownlint Ignore Configuration

## 1. Contexto

A pipeline de CI (*Plugin Quality CI*) estava falhando consistentemente na etapa de formatação e linting de Markdown (`npm run lint:md`). O motivo central era que o comando `markdownlint-cli2 "**/*.md"` estava capturando e validando arquivos `.md` localizados na pasta `node_modules`.

Como a pasta `node_modules` contém pacotes de terceiros, validar seu conteúdo gerava falsos-positivos na nossa pipeline, além de aumentar desnecessariamente o tempo de execução do processo de lint.

## 2. Decisão

Decidimos utilizar o arquivo `.markdownlint-cli2.jsonc` na raiz do projeto para excluir de forma definitiva diretórios gerados ou de dependências de terceiros.

Os seguintes diretórios foram incluídos no `"ignores"`:

- `node_modules`
- `dist`
- `build`
- `coverage`

### Por que não adicionar as flags no `package.json`?

A documentação do `markdownlint-cli2` suporta a passagem de globs ignorados diretamente no comando (ex: `"#node_modules"`). Contudo, optamos pelo `.markdownlint-cli2.jsonc` (já que a versão mais recente da CLI prefere este arquivo nativamente em vez do `.markdownlintignore`). Isso:

1. Mantém o `package.json` limpo e fácil de ler.
2. Centraliza as exclusões em um formato nativo da ferramenta.
3. Garante que os ignores funcionem consistentemente também quando o CLI do markdownlint for executado manualmente ou por extensões integradas nas IDEs, não ficando acoplado apenas aos scripts do npm.

## 3. Consequências

- A build de CI passa a rodar de forma bem sucedida sem conflitos de `node_modules`.
- Integrações locais do linter (no VSCode e através do lint-staged do Husky) não validarão a documentação de dependências instaladas.
- Caso surjam novas pastas de build ou de relatórios no futuro, a convenção será simplesmente anexá-las à propriedade `"ignores"` deste novo arquivo `.markdownlint-cli2.jsonc`.
