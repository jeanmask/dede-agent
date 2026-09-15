# Especificação de Design: Documentação Universal e Interoperabilidade (README.md)

- **Data:** 2026-09-14
- **Iniciativa:** Aprimoramento do README.md para Interoperabilidade, Copilot CLI, e Exemplos Práticos
- **Localização Alvo:** `/home/jean/.gemini/config/plugins/design-docs/README.md`
- **Status:** Aprovado

---

## 1. Visão Geral
O objetivo deste aprimoramento é transformar o `README.md` do plugin `design-docs` em um hub de documentação "Universal Hub". O README não apenas ensinará o uso nativo no Antigravity, mas também como utilizar os artefatos do plugin (`AGENTS.md` e `templates/`) como "Single Source of Truth" em outras plataformas de IA (Copilot CLI, GitHub Copilot, Cursor).

## 2. Estrutura Proposta para o README.md

O arquivo `README.md` será sobrescrito com a seguinte estrutura em Markdown:

### Título e Descrição Breve
- Nome: **Antigravity Design Docs Plugin**
- Badge de Licença MIT.
- Descrição de 2 linhas focada na governança arquitetural e templates agnósticos.

### Instalação
- Instrução de clonagem/cópia para o diretório de plugins do Antigravity (`~/.gemini/config/plugins/design-docs/`).

### Uso Nativo (Antigravity)
- Exemplificação dos slash commands nativos:
  - `/design-doc:create`
  - `/design-doc:review`

### Exemplos Práticos: Customização BYOT (Bring Your Own Template)
- Exemplo prático (snippet de código) mostrando como um projeto consumidor pode sobrescrever regras locais criando o arquivo `.agents/design-doc.yaml`.
- Exemplo do YAML definindo o `profile: "ai_genai"` e um caminho `template_path`.

### Interoperabilidade (O Plugin como Provedor Universal de Regras)
Esta seção documentará como consumir a governança do plugin em outras IAs:
- **GitHub Copilot CLI (Terminal)**: Exemplo prático de criação de um *alias* bash/zsh que injeta o `AGENTS.md` e o `default_pt.md` no prompt do CLI (ex: `gh copilot suggest`).
- **GitHub Copilot (IDE)**: Instrução rápida ensinando a referenciar o `AGENTS.md` dentro de um `.github/copilot-instructions.md` via instrução de leitura.
- **Cursor / Claude**: Menção sobre o uso do conteúdo de `/rules/AGENTS.md` injetado no `.cursorrules` ou `CLAUDE.md`.

### Licença
- Declaração explícita de uso livre via **MIT License**.

---

## 3. Considerações de Design
- **Single Source of Truth**: Ferramentas de terceiros devem ser instruídas a "ler" o arquivo `rules/AGENTS.md` do plugin, em vez de duplicar as regras em seus próprios arquivos, garantindo que qualquer mudança na governança seja refletida automaticamente no Copilot ou Cursor.
- **Enxuto e Acionável**: O README deve favorecer blocos de código copiáveis (`bash` ou `yaml`) em vez de longos parágrafos teóricos.
