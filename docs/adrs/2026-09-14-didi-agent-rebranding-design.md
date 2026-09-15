# Especificação de Design: Rebranding para "Didi Agent"

- **Data:** 2026-09-14
- **Iniciativa:** Rebranding do Plugin para "Didi Agent", focando em Persona, UX e Nomenclatura Open-Source.
- **Localização Alvo do Spec:** `/home/jean/.gemini/config/plugins/design-docs/docs/superpowers/specs/2026-09-14-didi-agent-rebranding-design.md`
- **Status:** Aprovado / Pronto para Implementação

---

## 1. Visão Geral

Esta especificação define o processo de refatoração e reposicionamento do plugin genérico "Design Docs" para o **"Didi Agent"**. A estratégia unifica uma marca carismática ("Didi: The Design Doc Assistant") com princípios fortes de UX, mantendo comandos de invocação puramente funcionais.

## 2. Decisões Arquiteturais e de Produto

### 2.1. Marca, Persona e Repositório

- **Nome Oficial do Repo**: `didi-agent`
- **Display Name**: "Didi Agent (Design Docs)"
- **Persona**: Um assistente autônomo e descontraído, mas altamente rigoroso com padrões arquiteturais.
- **Diretório do Plugin**: A pasta física será renomeada de `plugins/design-docs` para `plugins/didi-agent`.

### 2.2. Slash Commands (A Regra de Intuição de UX)

Conforme alinhamento de usabilidade (discoverability), as skills **não sofrerão alteração em seus nomes YAML**, mantendo a invocação pela intenção de uso:

- `/design-doc:create`
- `/design-doc:review`
- `/design-doc:export`

### 2.3. Integrações de Interoperabilidade

Os tutoriais de interoperabilidade (como o CLI Alias) adotarão a marca:

- **CLI Alias**: Recomendaremos a criação do alias `didi` no terminal (ex: `alias didi="gh copilot suggest..."`).
- **Arquivo de BYOT**: Projetos que consomem o Didi definirão suas regras no arquivo `.agents/didi.yaml` (em substituição ao antigo `design-doc.yaml`).

## 3. Escopo de Alterações Técnicas

A implementação deverá englobar:

1. **Renomeação de Diretório Base**: Mover `plugins/design-docs` para `plugins/didi-agent`.
2. **Atualização do `plugin.json`**: Mudar `name` para `didi-agent` e descrição refletindo a nova persona.
3. **Refatoração do `README.md`**:
   - Substituir as referências genéricas por "Didi Agent".
   - Atualizar os exemplos de arquivo de configuração para `.agents/didi.yaml`.
   - Atualizar a instrução de instalação (git clone) para refletir o novo nome do repositório local.
   - Atualizar o alias sugerido para `alias didi`.
4. **Skills e Referências Internas**:
   - Atualizar qualquer menção interna de caminhos nos arquivos `SKILL.md` ou `AGENTS.md` que referenciem o diretório `design-docs` antigo, trocando-os para `didi-agent`.
