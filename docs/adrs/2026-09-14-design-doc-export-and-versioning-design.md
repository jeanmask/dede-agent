# Especificação de Design: Hub de Publicação Universal, Source Maps e Versionamento Semântico

- **Data:** 2026-09-14
- **Iniciativa:** Criação da Skill `/design-doc:export`, Roteamento Multiplataforma (Notion/Confluence) e Governança de Estado
- **Localização Alvo do Spec:** `/home/jean/.gemini/config/plugins/design-docs/docs/superpowers/specs/2026-09-14-design-doc-export-and-versioning-design.md`
- **Status:** Rascunho / Para Revisão

---

## 1. Visão Geral

A evolução corporativa do plugin `design-docs` requer que os documentos gerados não fiquem apenas em repositórios locais, mas que alcancem plataformas de consumo de negócios (Confluence, Notion, Google Drive) sem perder a **Single Source of Truth (SSOT)** (o Markdown canônico) e a rastreabilidade das revisões técnicas.

Esta expansão introduz um **Hub de Publicação**, um sistema de **Recuperação via Source Map (Base64)** e um **Versionamento Semântico Autônomo** gerido pelo agente.

---

## 2. A Nova Skill: `/design-doc:export`

Criaremos a skill `skills/export/SKILL.md` que atuará como tradutora e roteadora.

### 2.1. Configuração de Destino (`.agents/design-doc.yaml`)

A configuração híbrida será expandida para suportar roteamento.

```yaml
storage:
  primary: "git_remote" # SSOT: Repositório central de arquitetura
  path: "./docs/adrs"
publishing:
  provider: "confluence" # "confluence", "notion", "gdrive", "git_remote"
  diagram_syntax: "plantuml" # "mermaid", "d2", "plantuml"
  target_auth:
    space_key: "ARCH"
```

### 2.2. Tradução de Formatos (Markdown Flavors)

O Agente na skill `export` fará a tradução inteligente do Markdown canônico para a plataforma de destino:

- **Confluence**: Conversão de tags genéricas para macros XHTML de Confluence e substituição de Mermaid por PlantUML (se configurado).
- **Notion**: Adaptação para blocos estruturais do Notion via API (remoção de HTML puro não suportado).
- **D2 / Mermaid**: Conversão semântica e bidirecional de fluxos de arquitetura.

### 2.3. Motor de Entrega (Delivery Automático)

O envio para as plataformas será feito respeitando a ordem de capacidade do ambiente:

1. **MCP (Model Context Protocol)**: Se o Antigravity tiver servidores como `mcp-confluence` ou `mcp-notion` configurados, a skill utilizará as tools MCP diretamente.
2. **REST API Scripts**: Se o MCP não estiver presente, a skill gerará um script bash/python no diretório `/scratch` para fazer o upload via chamadas REST HTTP (lendo tokens das variáveis de ambiente local).

---

## 3. Governança de Estado e Arquitetura

### 3.1. O Padrão "Source Map" Oculto (Recuperação de Estado)

É proibido que a exportação para Notion ou Confluence destrua o código base estruturado.

- A skill `export` obrigatoriamente codificará o Markdown original (Canônico) em base64.
- Injetará no rodapé do documento exportado (como comentário HTML) a payload:
  `<!-- design-doc-canonical-source: base64(VGhpcyBpcyBhbi...) -->`
- Se o usuário precisar editar um Design Doc a partir do Notion no futuro, a skill de criação/revisão saberá ler essa payload e restaurar o contexto perfeito de arquitetura.

### 3.2. Versionamento Semântico Autônomo (Agent-Driven)

A estrutura base do template (`templates/default_pt.md`) passará a ter metadados de versão no cabeçalho e um Changelog tabular:

```markdown
**Status**: 🟡 Em Revisão
**Versão do Documento**: `v1.2.0`

### Histórico de Alterações
| Versão | Data | Alteração | Motivo / Revisor |
|---|---|---|---|
| `v1.1.0` | 2026-09-14 | Adicionado Redis | Apontamento da revisão do time de BD. |
```

**Regra do Agente**: A skill `/design-doc:create` (no modo evolução) será programada para:

1. Sempre que o usuário solicitar uma alteração baseada em comentários ou apontamentos externos, o agente deverá incrementar o `Minor` (nova funcionalidade) ou `Patch` (correção) da versão no cabeçalho.
2. Adicionar uma nova linha à tabela de Changelog registrando quem pediu e por que foi alterado.

### 3.3. Proteção Single Source of Truth (SSOT)

A skill `export` aplicará uma verificação (Hard Gate): Nunca exportará um documento para o Confluence ou Notion sem antes garantir que a versão mais recente foi persistida fisicamente na plataforma primária de `storage` (ex: Git Repo local).
