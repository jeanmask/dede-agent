# Especificação de Design: Arquitetura de CI/CD e Inicialização do Repositório

- **Data:** 2026-09-14
- **Iniciativa:** Inicialização Git, Linting (YAML/Markdown) e Validação Estrutural (JSON Schema) para o Plugin Open-Source
- **Localização Alvo do Spec:** `/home/jean/.gemini/config/plugins/design-docs/docs/superpowers/specs/2026-09-14-cicd-architecture-design.md`
- **Status:** Rascunho / Para Revisão

---

## 1. Visão Geral

Como este plugin agora é um projeto open-source que receberá contribuições externas, precisamos garantir que as regras, os templates (Markdown) e as configurações (YAML) não sejam corrompidos acidentalmente. Esta especificação define a fundação de repositório (Git) e a pipeline de CI/CD via GitHub Actions.

## 2. Inicialização do Repositório (Fundação)

Como o diretório atual não é um repositório Git formal, a primeira etapa da implantação estabelecerá a raiz técnica:

- Execução de `git init` na raiz do plugin (`/home/jean/.gemini/config/plugins/design-docs`).
- Criação de um arquivo `.gitignore` padrão (ignorando pastas temporárias como `/scratch`, `.DS_Store`, etc).
- Realização do **Commit Inicial** (First Commit) para consolidar a estrutura arquitetural criada até o momento.

## 3. Arquitetura do GitHub Actions

O pipeline residirá em `.github/workflows/ci.yml` e será disparado nos eventos `push` (para a branch `main`) e `pull_request` (para controle de qualidade em contribuições). O pipeline é composto por 3 *Jobs* executados em paralelo:

### Job 1: Markdown Linting

- **Ferramenta:** `markdownlint-cli2-action`
- **Alvo:** Valida todos os arquivos `*.md` (incluindo `README.md`, `SKILL.md` das skills, e `default_*.md` dos templates).
- **Regras:** Valida sintaxe correta de cabeçalhos, quebras de linha e blocos de código.

### Job 2: YAML Linting

- **Ferramenta:** `action-yamllint`
- **Alvo:** Valida a sintaxe de todos os arquivos `*.yaml` e `*.yml`.
- **Regras:** Garante que a estrutura chave-valor do YAML está semanticamente bem formada (sem tabs onde deveria ser espaço, aspas não fechadas, etc).

### Job 3: Schema Validation (O Coração da Regra)

- **Artefato Novo:** Será criado o arquivo `templates/config.schema.json`.
- **Conteúdo do Schema:** Define as regras de negócio estruturais. Exige a existência do bloco `publishing`, do array de `profiles`, do atributo `version`, etc.
- **Ferramenta no CI:** Um simples executor Node.js rodando `ajv-cli` (ou equivalente em Python com `jsonschema`) para injetar o `config.yaml` contra o `config.schema.json`. Se o YAML não obedecer ao contrato do Schema, o Pull Request será bloqueado.

## 4. Impacto e Segurança Open-Source

Esta arquitetura atua como um "Hard Gate". A partir de sua implementação, é impossível que uma contribuição quebre o parser do Antigravity por um erro de digitação estrutural (`profles` vs `profiles`) ou corrompa a exibição visual dos templates Markdown.
