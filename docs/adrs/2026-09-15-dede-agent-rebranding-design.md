# Especificação de Design: Rebranding para "Dede Agent"

- **Data:** 2026-09-15
- **Iniciativa:** Pivotar a identidade visual e nome do agente de "Dede" para "Dede".
- **Localização Alvo:** `/home/jean/.gemini/config/plugins/dede-agent/docs/adrs/2026-09-15-dede-agent-rebranding-design.md`
- **Status:** Aprovado / Pronto para Implementação

---

## 1. Visão Geral
Decidimos evoluir a nomenclatura do mascote e agente. O projeto passa a se chamar **Dede Agent**. As diretrizes de separação entre marca e funcionalidade permanecem intactas (os comandos do Antigravity não mudam).

## 2. Alterações de Escopo
1. **Diretório e Repositório**: Renomear fisicamente a pasta de `dede-agent` para `dede-agent`. Atualizar symlinks.
2. **Documentação e Metadados**: Find-and-Replace rigoroso trocando "Dede" por "Dede" (respeitando capitalização) em:
   - `README.md`
   - `README-en.md`
   - `plugin.json`
3. **Interoperabilidade**:
   - Arquivo padrão sugerido: `.agents/dede.yaml`.
   - Alias do Copilot CLI: `alias dede="..."`.
4. **Skills Intocadas**: Os arquivos YAML/Markdown das skills continuarão utilizando o identificador `/design-doc:*`.
