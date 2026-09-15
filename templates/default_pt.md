Template Version: v1.0 | Profile: {{profile_name}}

# [Nome do Projeto]

**Status**: [🟡 Rascunho | 🔵 Em Revisão | 🟢 Aprovado]
**Versão do Documento**: `v1.0.0`

### Histórico de Alterações

| Versão | Data | Alteração | Motivo / Revisor |
|---|---|---|---|
| `v1.0.0` | YYYY-MM-DD | Criação Inicial | Draft base submetido. |

---

### 1. Equipe & Revisores [OBRIGATÓRIO]

- **Autores**: [Nome - Squad]
- **Revisores**: [Nome - Squad / Arquitetura]

### 2. Visão Geral & Documentos de Referência [OBRIGATÓRIO]

- **Documentos de Referência**: [PRD / Spec Funcional / ADRs / Design Docs Anteriores]
- Resumo técnico executivo em até 2 parágrafos.

### 3. Contexto & Diagnóstico Técnico [OBRIGATÓRIO]

- Problema de engenharia, limitações da stack vigente e débitos técnicos atuais.

### 4. Objetivos da Iniciativa [OBRIGATÓRIO]

#### 4.1. Objetivos de Negócio

- Metas estratégicas amplas (ex: Aumentar conversão).

#### 4.2. Objetivos Técnicos & SLOs

- Métricas mensuráveis (ex: p99 < 120ms, tolerância a partições). Proibido inserir regras ou lógicas aqui.

### 5. Fora de Escopo [OBRIGATÓRIO]

- O que não será entregue tecnicamente nesta fase.

### 6. Solução Existente

#### 6.1. Arquitetura Atual

- Diagrama C4 Container Nível 2 evidenciando os sistemas que hoje sustentam o processo.

#### 6.2. Fluxos Atuais

- Diagrama de sequência ilustrando gargalos atuais.

### 7. Solução Proposta [OBRIGATÓRIO]

#### 7.1. Regras Objetivas da Solução

- Regras determinísticas de domínio, condicionais de negócio, garantias e invariantes (ex: idempotência).

#### 7.2. Arquitetura Proposta

- C4 Container Nível 2 (Agnóstico a repositórios, expandindo os sistemas afetados).

#### 7.3. Recursos Provisionados

- Tabela de recursos em nuvem ou infraestrutura base.

#### 7.4. Fluxos Propostos

- Diagrama de sequência com `autonumber` e tratamento de erros via `alt/else`.

#### 7.5. Interfaces / Telas [OPCIONAL]

- Desenho de telas ou fluxos de UX.

#### 7.6. Contratos e Payloads

- Assinatura de APIs (REST, gRPC) ou tópicos de eventos.

### 8. Plano de Implementação & Rollout Cross-Repo [OBRIGATÓRIO]

- Ordem de execução: Fase 1 (Infra), Fase 2 (Produtores), Fase 3 (Consumidores), Fase 4 (Canary/Rollout).
- Plano de rollback cross-repo com garantias de retrocompatibilidade.

### 9. Soluções Alternativas e Trade-offs [OBRIGATÓRIO]

- Caminhos técnicos avaliados e descartados.

### 10. Testabilidade e Observabilidade [OBRIGATÓRIO]

- Testes unitários/integração.
- OpenTelemetry, Métricas RED, Logs e Alertas.

### 11. Impactos Cross-Squad e Catálogo de Repositórios [OBRIGATÓRIO]

| Serviço / Repositório | Squad Responsável | Caminho / URL | Mudança Prevista | Dependência de Deploy |
|---|---|---|---|---|
| repo-x | Squad Y | url | Mudança | Fase Z |

### 12. Segurança e Privacidade [OBRIGATÓRIO]

- **Identidade e Acesso**: OIDC, RBAC.
- **Proteção de Dados**: Vault, WAF, Criptografia.
- **Privacidade e Governança**: LGPD/GDPR.

### 13. Matriz de Riscos e Mitigações

- Categoria (Arquitetura/Segurança), Risco, Impacto e Mitigação.

### 14. Perguntas em Aberto

- Pontos que precisam ser esclarecidos.

### 15. Referências e Links Úteis

- GitHub, Jira, Wikis.
