# Especificação de Design: Plugin Open-Source `design-docs` com Templates Dinâmicos, Suporte a PRD e IA

- **Data:** 2026-09-14
- **Nome da Iniciativa:** Transformação em Plugin de Design Docs (Open-Source), Templates Híbridos, Multi-Repo, Perfil de IA e Skill de Revisão
- **Localização Alvo:** `/home/jean/.gemini/config/plugins/design-docs/`
- **Status:** Aprovado / Pronto para Implementação

---

## 1. Visão Geral e Motivação (Foco Open-Source)

A capacidade de gerar Design Docs corporativos começou como uma skill isolada. Com a decisão de tornar a solução um **Plugin Open-Source** compatível com múltiplas plataformas e ecossistemas (GitHub, GitLab, Jira, Linear, etc.), a arquitetura precisou evoluir para ser extensível, agnóstica a empresas (remoção de referências proprietárias) e modular.

**Principais Motivadores da Reestruturação:**

1. **Distribuição Open-Source**: O plugin deve ser instalável em qualquer ambiente Antigravity, suportando múltiplos idiomas (i18n) e permitindo que a comunidade (ou empresas) crie e compartilhe seus próprios perfis de template.
2. **Templates Dinâmicos e Extensibilidade (BYOT)**: Empresas possuem formatos diferentes. A solução adota um sistema híbrido onde o template base pode ser sobrescrito pelo repositório local (`.agents/design-doc.yaml`).
3. **Ingestão Inteligente (Zero a 100)**: Do suporte a PRDs/Specs estruturados até um Modo Entrevista (para quem não tem PRD) e a evolução de Design Docs Anteriores.
4. **Adoção de Padrões de IA (GenAI)**: Inclusão nativa de um perfil para projetos de Inteligência Artificial, tratando versionamento de prompts, LLMOps, RAG e AI Safety.
5. **Auditoria Contínua**: Introdução da skill de `review` para auditar documentos historicamente baseada na *versão do template* utilizada.

---

## 2. Arquitetura do Plugin e Estrutura de Diretórios (Padrão OSS)

A estrutura adere às convenções do Antigravity Customization System e às boas práticas de projetos open-source:

```text
~/.gemini/config/plugins/design-docs/
├── plugin.json                 # Manifesto oficial do plugin
├── README.md                   # Instruções de instalação, uso e contribuição OSS
├── rules/
│   └── AGENTS.md               # Governança arquitetural global e C4
├── templates/
│   ├── config.yaml             # Configuração declarativa de perfis (standard, ai_genai, etc.)
│   ├── default_en.md           # Template markdown base (Inglês - i18n)
│   └── default_pt.md           # Template markdown base (Português - i18n)
├── skills/
│   ├── create/
│   │   └── SKILL.md            # Skill `design-doc:create`
│   └── review/
│       └── SKILL.md            # Skill `design-doc:review`
└── docs/
    └── superpowers/specs/      # Histórico de arquitetura do próprio plugin
```

---

## 3. Especificação dos Componentes

### 3.1. Manifesto do Plugin (`plugin.json`)

```json
{
  "name": "design-docs",
  "description": "Open-Source plugin for creating, reviewing, and governing Technical Design Docs",
  "version": "1.0.0",
  "repository": "https://github.com/org/design-docs-plugin"
}
```

### 3.2. Sistema Híbrido de Templates e Perfis

Resolução em **3 camadas**, ideal para adoção corporativa descentralizada:

#### A. Configuração Base do Plugin (`templates/config.yaml`)

Define os perfis prontos para uso:

- **`standard`**: Completo (arquitetura, infra, segurança, rollout).
- **`lightweight`**: Ágil (remove dependências complexas e alternativas rígidas).
- **`ai_genai`** (Inteligência Artificial): Adiciona blocos modulares críticos para IA.

#### B. Sobrescrita no Workspace (`.agents/design-doc.yaml`)

Permite que o usuário defina regras próprias no seu projeto e adote a abordagem **BYOT (Bring Your Own Template)**, apontando para um markdown customizado:

```yaml
profile: "custom"
template_path: "./docs/templates/meu_template_corporativo.md"
language: "pt-BR"
```

#### C. Versionamento do Template (Proteção Histórica)

Todo Design Doc gerado deverá possuir um cabeçalho fixo: `Template Version: v1.0 | Profile: [nome_do_perfil]`. Isso garante que a skill de `review` audite o documento com as regras de quando ele foi criado, evitando quebras futuras se o `config.yaml` evoluir.

---

### 3.3. Template Base: Agnóstico e Padrão de Indústria

A estrutura do template (seja en ou pt) removeu qualquer acoplamento a ferramentas proprietárias (NOC 24x7 interno, e-mails hardcoded). A nova base adota os pilares da engenharia moderna:

- **Separação Categórica**: "Objetivos Estratégicos & SLOs" (Visão macro) isolados de "Regras Objetivas de Solução" (Lógicas e contratos).
- **Segurança em 3 Pilares**: Identidade/Acesso (OIDC/RBAC), Proteção de Dados (Vault/Criptografia) e Privacidade/LGPD/GDPR.
- **Observabilidade**: OpenTelemetry, logs estruturados e métricas RED/USE.
- **Governança Multi-Repo**: Diagramas C4 estritamente puros (sem URLs de git ou squads nos nós). O mapeamento de repositórios fica em uma tabela consolidada na Seção 11.

---

### 3.4. O Perfil de IA & GenAI (`ai_genai`)

Um diferencial do plugin open-source é trazer padrões maduros para engenharia de IA. Ao ativar este perfil, o template ganha seções mandatórias de:

1. **Modelagem, RAG e Ciclo de Vida de Prompts**: Provedores primários/fallback, chunking, banco vetorial e, criticamente, **onde os prompts residem e como são versionados/deployados**.
2. **Segurança, Evals e Privacidade**:
   - Defesa contra Prompt Injection e Jailbreak;
   - Opt-out de Treinamento (Garantia de que provedores de nuvem não treinam em cima dos dados) e Residência de Dados (Data Residency);
   - Framework de Avaliação Contínua (Golden datasets, métricas de hallucination).
3. **UX Fallback e Degradação Graciosa**: O que acontece na interface quando o LLM falha ou o filtro de segurança barra a resposta.
4. **FinOps de IA (LLMOps)**: Custos estimados de tokens, caching e TTFT (Time To First Token).

---

### 3.5. Skills do Plugin

#### Skill `design-doc:create`

- **Discoverability / onboarding**: Se iniciada no modo Entrevista (sem PRD), o agente apresenta ativamente os perfis disponíveis (`standard`, `lightweight`, `ai_genai`) antes de prosseguir.
- **Multi-Entrada**: Ingestão fluida de PRDs (traduzindo produto em contexto de engenharia sem cópia literal) ou ingestão de Design Docs Anteriores (para evolução/Fase 2).
- **Integração Universal via Handoff (Upstream Skills)**: A skill atua como a *Fase 2* (Formalização) para qualquer skill de ideação prévia (ex: `superpowers:brainstorming`, `grill-me`).
  - Lê o artefato final gerado ou o consenso do histórico de conversa como se fosse o PRD virtual;
  - Se a ideia estiver abstrata demais ou sem consenso técnico pré-estabelecido, a skill pausa a formalização e recomenda o uso de skills de ideação (como o `grill-me`) antes de desenhar a arquitetura dura.
- **Suporte Multilíngue (i18n)**: Detecta o idioma do prompt ou do workspace (pt-BR/en-US) e utiliza o respectivo template base.

#### Skill `design-doc:review`

- **Auditoria Histórica e Contextual**: Lê a tag `Template Version` e o `Profile` no cabeçalho do arquivo para aplicar as regras da época de geração.
- **UX do Revisor**: A saída gera um **Checklist em Markdown (`- [ ]`)** diretamente acionável e enumerando os débitos arquiteturais. Isso permite copiar o feedback para plataformas como GitHub Issues ou Jira de forma amigável.

---

## 4. Distribuição Open-Source e Documentação

Sendo um projeto livre e comunitário, o repositório adotará padrões rigorosos de acessibilidade e distribuição:

1. **Licenciamento Aberto**: Inclusão de licença open-source permissiva, livre e gratuita (Licença MIT), permitindo que empresas adotem e customizem o plugin internamente sem entraves jurídicos.
2. **Documentação Enxuta (README.md)**: Um `README.md` direto ao ponto, não excessivamente longo, cobrindo exclusivamente:
   - O que o plugin faz de forma objetiva;
   - Como instalar localmente ou via registro em `plugins.json`;
   - Como invocar a skill de `create` e `review`;
   - Como customizar os templates (BYOT) via `.agents/design-doc.yaml`.
3. **Estrutura Base**: Setup do repositório em `/home/jean/.gemini/config/plugins/design-docs/` contendo `LICENSE`, `README.md`, `plugin.json` e as subpastas estruturais.
4. **Motor de Templates e Skills**: Codificação do `templates/config.yaml`, templates markdown (`en` e `pt`), e prompts das skills respeitando a dinâmica de handoff universal.
