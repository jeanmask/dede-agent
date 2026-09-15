# ADR 006: Commitlint e Release Automático (Trunk-Based)

**Status:** Aceito
**Data:** 2026-09-15

## Contexto e Problema

Com a padronização das ferramentas para o ecossistema Node (Husky), percebeu-se a necessidade de também padronizar as mensagens de commit (Conventional Commits).
No modelo Trunk-Based Development com CI/CD, é crucial ter rastreabilidade clara sobre quais commits introduzem novas features (`feat`), quais resolvem bugs (`fix`) e quais são apenas manutenção (`chore`).
Isso permite a automatização segura do cálculo da próxima versão SemVer (Major, Minor, Patch).

## Decisão

Adotamos a seguinte arquitetura no `package.json` baseada nas ferramentas mais modernas do ecossistema JavaScript:

1. **Commitlint:** Adicionado através dos pacotes `@commitlint/cli` e `@commitlint/config-conventional`. Intercepta mensagens de commit localmente via `husky` (`.husky/commit-msg`), bloqueando commits fora do padrão Angular.
2. **Release-it:** Ferramenta escolhida para automatizar a liberação de versões de forma semi-automática. Substitui soluções legadas como `standard-version` e previne o excesso de *tags* de um `semantic-release` agressivo.
3. **Changelog e Bump duplo:** O plugin `@release-it/conventional-changelog` interpreta o histórico e faz o bump da versão no `package.json` e gera as Release Notes no `CHANGELOG.md`.
4. **Hook de Sincronia:** Utilizamos o evento `after:bump` do `release-it` (via `sed`) para injetar a mesma versão no `plugin.json`, garantindo que ambos os manifestos fiquem sincronizados no momento do commit de release.

## Consequências

- **Positivas:** Geração automática e precisa de changelogs. Fim de discussões sobre qual deveria ser a próxima versão. Histórico de git limpo e semântico.
- **Negativas:** Desenvolvedores terão que se habituar à sintaxe do Conventional Commits e seus commits locais falharão caso esqueçam de prefixá-los (ex: `feat:`, `fix:`).
