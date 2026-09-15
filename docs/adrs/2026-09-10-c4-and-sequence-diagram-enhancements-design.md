# Especificação de Design: Aprimoramento da Skill design-doc com Diretrizes C4 e Sequência

- **Data:** 2026-09-10
- **Nome da Iniciativa:** Evolução da Skill `design-doc:create`
- **Arquivo Alvo:** `/home/jean/.gemini/config/skills/design-docs/SKILL.md`
- **Status:** Concluído / Implementado

---

## 1. Visão Geral e Motivação

A skill `design-doc:create` define o comportamento do agente especialista responsável pela elaboração de Design Docs corporativos. Durante a criação do documento de arquitetura e governança do projeto `akamai-edge-blueprint`, foram estabelecidos padrões de alta maturidade para diagramas arquiteturais e de fluxo que se mostraram essenciais para a clareza técnica e aprovação em comitês.

Entretanto, a especificação anterior no arquivo `SKILL.md` trazia instruções genéricas para diagramas (ex: apenas mencionava *"C4 Container para visão estrutural e sequenceDiagram para fluxos de integração"*), o que frequentemente induzia o modelo a gerar diagramas simplistas demais, sem expansão de contêineres, sem protocolos nas setas e sem distinção clara entre sistemas internos e externos.

Esta especificação consolida os ajustes mandatórios implementados no `SKILL.md` para garantir rigor técnico e padronização na geração de diagramas C4 Model e Diagramas de Sequência.

---

## 2. Requisitos e Diretrizes Arquiteturais

### 2.1. Premissas dos Diagramas C4 Model

1. **Presença Mínima Obrigatória**: Todo Design Doc deve conter **pelo menos um diagrama C4 Model**, com forte preferência para o **C4 Container (Nível 2)**. A inclusão do C4 Context (Nível 1) é opcional e recomendada quando for necessário evidenciar o ecossistema corporativo macro antes do detalhamento interno.
2. **Expansão Mandatória dos Sistemas Modificados**:
   - Todo `Software System` que sofrer modificação, adição de novos contêineres ou que seja o objeto principal da iniciativa deve ser expandido visualmente em seus contêineres através de blocos `subgraph` do Mermaid.
   - Sistemas adjacentes que apenas consomem ou fornecem dados sem sofrer alteração interna permanecem como caixas fechadas de sistema.
3. **Formatação Estrita de Metadados nos Nós**:
   - **Pessoa**: `Id["Nome da Persona / Papel<br/>[Person]<br/>Descrição das responsabilidades"]`
   - **Sistema Interno**: `Id["Nome do Sistema<br/>[Software System]<br/>Descrição da responsabilidade geral dentro da organização"]`
   - **Sistema Externo**: `Id["Nome do Provedor / Vendor<br/>[External System]<br/>Descrição da plataforma terceira externa (ex: Akamai, Stripe, AWS, Auth0)"]`
   - **Contêiner (dentro de subgraphs)**: `Id["Nome do Contêiner<br/>[Container: Tecnologia / Linguagem / Framework]<br/>Descrição detalhada da função específica"]`
4. **Relacionamentos e Setas com Protocolo**:
   - Toda conexão direcional entre nós deve explicitar a ação executada e o protocolo ou formato de transporte entre parênteses.
   - Formato padrão: `Origem -->|"Ação descrita (Protocolo/Formato: HTTPS/REST, gRPC, AMQP/Kafka, Git/SSH, SQL/TCP)"| Destino`.

### 2.2. Premissas dos Diagramas de Sequência

1. **Numeração Automática**: Uso mandatório de `autonumber` no início do bloco `sequenceDiagram`.
2. **Qualificação de Participantes**: Separação precisa entre `actor` (atores humanos) e `participant` (serviços ou contêineres com suas respectivas atribuições).
3. **Caminhos Alternativos e Falhas**: Inclusão obrigatória de blocos `alt / else` cobrindo cenários de sucesso (ex: HTTP 200/201) e cenários de exceção, recusa ou falha (ex: HTTP 400/401/403/500, timeouts, rejeição por validação/gate).
4. **Respostas e Payloads Expressos**: Mensagens de retorno devem apontar status, códigos de erro ou parâmetros principais trocados.

---

## 3. Detalhamento das Modificações no `SKILL.md`

### 3.1. Seção de Governança e Diretrizes Técnicas

Substituída a diretriz de diagramas por uma subseção normativa detalhada:

- Inclusão explícita das regras de modelagem C4 (Nível 2 preferencial, expansão em `subgraph`, metadados de 3 linhas `[Nome / Tipo: Tech / Descrição]`, diferenciação `[Software System]` vs `[External System]`, e protocolos nas setas).
- Inclusão das regras de diagramas de sequência (`autonumber`, `alt/else`, mapeamento de status e participantes).

### 3.2. Seção 6.1 e 6.2 (Solução Existente no Template)

- Atualizada a instrução da seção `6.1. Arquitetura` orientando a criação de C4 Container representando a arquitetura legada/atual, expandindo os sistemas que possuem código/componentes atuais.
- Atualizada a seção `6.2. Fluxos` instruindo o `sequenceDiagram` com `autonumber` e `alt/else` mostrando as falhas ou limitações do fluxo atual.

### 3.3. Seção 7.2 e 7.4 (Solução Proposta no Template)

- Atualizada a seção `7.2. Arquitetura` com instrução e estrutura de exemplo para C4 Container (Nível 2), onde os sistemas com novos componentes são agrupados em `subgraph SystemX["Sistema: ..."]`, os novos contêineres trazem suas tecnologias (`[Container: Node.js / Express]`, `[Container: Terraform / HCL]`, etc.), sistemas de terceiros são identificados com `[External System]`, e as setas contêm `(Protocolo: ...)`.
- Atualizada a seção `7.4. Fluxos` com modelo de `sequenceDiagram` contendo `autonumber`, notas de gates/validações (`Note over ...`), e blocos `alt / else` para cenários de aprovação/sucesso e rejeição/erro.

---

## 4. Validação e Critérios de Aceite

1. **Sintaxe e Integridade**: O arquivo `/home/jean/.gemini/config/skills/design-docs/SKILL.md` permanece com YAML frontmatter válido (`name: design-doc:create`, `description: ...`) e Markdown consistente.
2. **Exemplos Transparentes**: As instruções adicionadas são inequívocas para qualquer instância de LLM que carregar a skill.
3. **Aderência ao Design Doc de Referência**: A formatação C4 e de sequência gerada pela skill reflete a qualidade e o nível de detalhes presente no `akamai-edge-blueprint/docs/design-doc.md`.
